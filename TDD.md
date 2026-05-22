# Technical Design Document (TDD)
## Project: HearthGuild Village Discord Bot
## Language: Python (async)
## Revision: v1.0

---

## 1. Executive Summary

This document defines an implementation-ready architecture for a Discord economy bot with a simulation-driven village economy. The design prioritizes:

- deterministic and auditable economy logic,
- stable currency behavior,
- thin command handlers,
- transactional safety,
- exploit resistance,
- scalable async services.

Command architecture rule:
- command -> service -> repository/database
- never embed business logic directly in command handlers.

---

## 2. Technology Stack

Required:
- Python 3.12+
- discord.py 2.x
- PostgreSQL 15+
- SQLAlchemy 2.x (async ORM/core)
- Alembic
- Redis 7+
- APScheduler or internal async scheduler

Recommended libraries:
- pydantic v2 for DTO validation
- structlog or loguru for structured logging
- prometheus-client for metrics
- tenacity for retry policies
- orjson for high-speed serialization

---

## 3. High-Level Architecture

## 3.1 Logical Components

1. Discord Interface Layer
- Cogs, slash commands, interaction responses.
- Converts user input to service requests.

2. Application Service Layer
- ProfessionService
- MarketService
- EconomyService
- ContractService
- ReputationService
- HousingService
- EventService
- AntiExploitService
- BenefitShopService
- WebPortalService

3. Domain Engine Layer
- PricingEngine
- InflationEngine
- ScarcityEngine
- ReputationEngine
- TrustEngine

4. Data Access Layer
- Repositories with SQLAlchemy async sessions.
- Unit-of-work transaction boundaries.

5. Infrastructure Layer
- Redis cache, distributed locks, job scheduler, metrics, tracing.

6. Web Platform Layer (Main Bot Website)
- Public site pages (landing, wiki, event calendar).
- Authenticated player portal (profile, market board, role/perk shop, trust summary).
- Admin configuration portal (shop catalog, role mapping, feature flags).
- API gateway sharing the same domain services and persistence layer.

### 3.2 Event-Driven Flow

Domain events emitted by services:
- ContractFulfilled
- TradeExecuted
- ItemCrafted
- ReputationChanged
- InflationThresholdCrossed
- EventStarted

Event handlers trigger:
- metric updates,
- cache invalidation,
- dynamic coefficient recalculation,
- notification fan-out.

---

## 4. Modular Folder Structure

```text
bot/
  app.py
  config/
    settings.py
    feature_flags.py
  interfaces/
    discord/
      cogs/
        economy_cog.py
        profession_cog.py
        market_cog.py
        benefits_cog.py
        housing_cog.py
        social_cog.py
        events_cog.py
      transformers/
      views/
      embeds/
  application/
    services/
      economy_service.py
      profession_service.py
      market_service.py
      benefit_shop_service.py
      role_sync_service.py
      contract_service.py
      reputation_service.py
      trust_service.py
      housing_service.py
      village_project_service.py
      seasonal_event_service.py
      web_portal_service.py
    dto/
      requests.py
      responses.py
    orchestrators/
      onboarding_orchestrator.py
      emergency_orchestrator.py
  domain/
    models/
      money.py
      item.py
      contract.py
      reputation.py
    engines/
      pricing_engine.py
      inflation_engine.py
      scarcity_engine.py
      balancing_engine.py
    policies/
      anti_exploit_policy.py
      taxation_policy.py
  infrastructure/
    db/
      base.py
      session.py
      models/
      repositories/
      migrations/  # alembic
    cache/
      redis_client.py
      cache_keys.py
    messaging/
      event_bus.py
      domain_events.py
    scheduler/
      jobs/
        market_reprice_job.py
        inflation_monitor_job.py
        seasonal_tick_job.py
        economy_snapshot_job.py
        benefit_expiry_job.py
    locks/
      distributed_lock.py
    telemetry/
      metrics.py
      logging.py
      tracing.py
  web/
    app.py
    api/
      routes/
        auth_routes.py
        player_routes.py
        market_routes.py
        benefit_shop_routes.py
        admin_shop_routes.py
      dependencies.py
      middleware.py
    templates/
    static/
    frontend/
      src/
      dist/
  tests/
    unit/
    integration/
    load/
    fixtures/
```

---

## 5. Data Model and Schema Design

## 5.1 Core Tables

### users
- id (PK, bigint)
- discord_id (unique, bigint)
- display_name (varchar)
- joined_at (timestamp)
- active_flag (bool)
- home_id (nullable FK -> housing.id)

Indexes:
- unique(discord_id)
- index(active_flag, joined_at)

### professions
- id (PK)
- code (unique)
- name
- description

### user_professions
- id (PK)
- user_id (FK users)
- profession_id (FK professions)
- rank (smallint)
- xp (int)
- specialization_code (nullable)
- mastery_efficiency (numeric)
- mastery_quality (numeric)
- mastery_community (numeric)
- updated_at

Constraints:
- unique(user_id, profession_id)
- check(rank between 1 and 5)

### items
- id (PK)
- code (unique)
- name
- category
- base_value (numeric)
- stack_limit (int)
- durability_flag (bool)
- tradable_flag (bool)

### inventory_slots
- id (PK)
- user_id (FK users)
- item_id (FK items)
- quantity (numeric)
- quality_tier (smallint)
- location_type (enum: personal, housing, workshop)
- locked_flag (bool)

Constraints:
- check(quantity >= 0)
- unique(user_id, item_id, quality_tier, location_type)

### crafting_recipes
- id (PK)
- output_item_id (FK items)
- output_quantity
- labor_cost
- profession_id (FK professions)
- min_rank
- seasonal_tag (nullable)

### recipe_inputs
- recipe_id (FK crafting_recipes)
- item_id (FK items)
- quantity
- PRIMARY KEY (recipe_id, item_id)

### housing
- id (PK)
- owner_user_id (FK users)
- home_type (enum: den, burrow, treehouse, cottage, workshop)
- level (smallint)
- storage_capacity (int)
- hosting_capacity (int)
- upkeep_cost_daily (numeric)
- comfort_score (numeric)

Indexes:
- unique(owner_user_id)

### market_listings
- id (PK)
- seller_user_id (FK users)
- item_id (FK items)
- quantity_remaining (numeric)
- unit_price (numeric)
- listing_type (enum: fixed, auction)
- status (enum: open, sold, cancelled, expired)
- created_at
- expires_at

Indexes:
- index(item_id, status, created_at)
- index(seller_user_id, status)

### market_trades
- id (PK)
- listing_id (nullable FK market_listings)
- buyer_user_id (FK users)
- seller_user_id (FK users)
- item_id (FK items)
- quantity
- unit_price
- fee_amount
- tax_amount
- executed_at

Indexes:
- index(item_id, executed_at)
- index(buyer_user_id, executed_at)
- index(seller_user_id, executed_at)

### market_price_history
- id (PK)
- item_id (FK items)
- bucket_ts (timestamp)
- avg_price (numeric)
- median_price (numeric)
- trade_volume (numeric)
- supply_index (numeric)
- demand_index (numeric)
- scarcity_score (numeric)

Constraints:
- unique(item_id, bucket_ts)

### contracts
- id (PK)
- issuer_type (enum: npc, player, village)
- issuer_user_id (nullable FK users)
- assignee_user_id (nullable FK users)
- item_id (FK items)
- quantity_required
- quality_required
- reward_currency
- reward_reputation
- escrow_amount
- due_at
- status (enum: open, accepted, fulfilled, failed, cancelled)

Indexes:
- index(status, due_at)
- index(assignee_user_id, status)

### trust_profiles
- user_id (PK FK users)
- trust_score (numeric)
- trust_tier (smallint)
- dispute_count_30d (int)
- fulfillment_rate_30d (numeric)
- endorsement_score (numeric)
- updated_at

### trust_reviews
- id (PK)
- reviewer_user_id (FK users)
- target_user_id (FK users)
- contract_id (nullable FK contracts)
- rating (smallint)
- review_tag (enum: reliability, quality, communication)
- weight (numeric)
- created_at

Constraints:
- check(rating between 1 and 5)
- unique(reviewer_user_id, target_user_id, contract_id, review_tag)

### reputation_profiles
- user_id (PK FK users)
- helpful_villager (numeric)
- trusted_trader (numeric)
- cozy_host (numeric)
- skilled_artisan (numeric)
- festival_organizer (numeric)
- updated_at

### village_projects
- id (PK)
- code (unique)
- name
- status (enum: planned, active, completed)
- contribution_deadline
- reward_policy_json (jsonb)

### village_project_contributions
- id (PK)
- project_id (FK village_projects)
- user_id (FK users)
- item_id (nullable FK items)
- item_quantity (numeric)
- labor_points (numeric)
- reputation_granted (numeric)
- contributed_at

Indexes:
- index(project_id, user_id)

### seasonal_events
- id (PK)
- code (unique)
- name
- starts_at
- ends_at
- config_json (jsonb)
- active_flag (bool)

### benefit_catalog
- id (PK)
- guild_id (bigint)
- code (varchar, unique per guild)
- benefit_type (enum: role, perk, cosmetic, access)
- discord_role_id (nullable bigint)
- display_name (varchar)
- description (text)
- price_currency (numeric)
- duration_days (nullable int)
- reputation_req (numeric)
- trust_tier_req (smallint)
- stock_limit (nullable int)
- active_flag (bool)
- sort_order (int)
- metadata_json (jsonb)

Constraints:
- unique(guild_id, code)
- check(price_currency >= 0)

### user_benefit_grants
- id (PK)
- guild_id (bigint)
- user_id (FK users)
- benefit_id (FK benefit_catalog)
- source (enum: shop, admin, event)
- status (enum: active, expired, revoked)
- granted_at (timestamp)
- expires_at (nullable timestamp)
- revoked_reason (nullable varchar)

Indexes:
- index(guild_id, user_id, status)
- index(benefit_id, status)
- index(expires_at)

### role_shop_orders
- id (PK, uuid)
- guild_id (bigint)
- user_id (FK users)
- benefit_id (FK benefit_catalog)
- price_paid (numeric)
- tax_paid (numeric)
- status (enum: pending, completed, failed, refunded)
- idempotency_key (varchar)
- correlation_id (varchar)
- created_at (timestamp)
- completed_at (nullable timestamp)

Constraints:
- unique(idempotency_key)

### guild_benefit_config
- guild_id (PK, bigint)
- shop_enabled (bool)
- max_active_benefits_per_user (int)
- monthly_top_tier_cap (int)
- inflation_price_link_enabled (bool)
- treasury_split_bps (int)
- starter_cosmetic_discount_pct (numeric)
- updated_at (timestamp)

### web_sessions
- id (PK, uuid)
- user_id (FK users)
- guild_id (bigint)
- oauth_provider (enum: discord)
- access_scope (varchar)
- created_at (timestamp)
- expires_at (timestamp)
- ip_hash (varchar)
- user_agent_hash (varchar)

Indexes:
- index(user_id, guild_id, expires_at)

### npc_trade_contracts
- id (PK)
- item_id (FK items)
- target_qty
- payout_multiplier (numeric)
- region_tag
- valid_from
- valid_to

### transactions_ledger
- id (PK, uuid)
- tx_type (enum: faucet, sink, trade, transfer, fee, tax, upkeep, reward)
- source_user_id (nullable FK users)
- target_user_id (nullable FK users)
- amount_currency (numeric)
- item_id (nullable FK items)
- item_qty (nullable numeric)
- idempotency_key (varchar)
- correlation_id (varchar)
- metadata_json (jsonb)
- created_at

Constraints:
- unique(idempotency_key)
- check(amount_currency >= 0)

### economy_snapshots
- id (PK)
- snapshot_ts
- total_currency (numeric)
- active_goods_value (numeric)
- money_supply_ratio (numeric)
- inflation_index (numeric)
- gini_like_index (numeric)
- transaction_velocity (numeric)
- ehi (numeric)
- notes

Constraints:
- unique(snapshot_ts)

---

## 6. Relationship Notes and Integrity Safeguards

- Monetary changes are append-only in transactions_ledger.
- User balances should be computed from ledger or maintained via strongly-consistent balance table with triggers + reconciliation.
- Contract fulfillment and payment occur in one DB transaction.
- Inventory quantity updates use row-level locks (SELECT FOR UPDATE).
- Use idempotency keys on all write commands from Discord interactions.

---

## 7. Service Layer Design

## 7.1 Command Handling Pattern

1. Cog validates input and auth context.
2. Cog calls application service method.
3. Service opens transaction and invokes domain engine.
4. Repository persists changes.
5. Service emits domain events.
6. Cog formats response embed.

### 7.2 Service Contracts (Examples)

MarketService:
- create_listing(user_id, item_id, qty, price, idempotency_key)
- buy_listing(buyer_id, listing_id, qty, idempotency_key)
- cancel_listing(user_id, listing_id)

BenefitShopService:
- list_offers(guild_id, user_id)
- purchase_benefit(guild_id, user_id, benefit_id, idempotency_key)
- revoke_benefit(grant_id, reason)
- sync_discord_role(grant_id)

WebPortalService:
- get_player_dashboard(guild_id, user_id)
- get_market_overview(guild_id)
- get_shop_config(guild_id, admin_user_id)
- update_shop_config(guild_id, payload, admin_user_id)

ContractService:
- accept_contract(user_id, contract_id)
- fulfill_contract(user_id, contract_id, provided_items, idempotency_key)

EconomyService:
- compute_snapshot()
- apply_stabilization_cycle()

ProfessionService:
- perform_labor(user_id, profession_code, action_code)
- craft_item(user_id, recipe_id, qty)

---

## 8. Economy Engine Algorithms

## 8.1 Pricing Engine (Per Item)

Definitions:
- P_base: baseline item value
- P_hist: EWMA of recent executed trade prices
- S: supply index
- D: demand index
- M_season: seasonal multiplier
- M_event: event multiplier
- M_scarcity: scarcity multiplier
- alpha: smoothing factor

Raw price:
- P_raw = (w1 * P_base + w2 * P_hist) * (D / max(S, epsilon)) * M_season * M_event * M_scarcity

Smoothed price:
- P_t = alpha * P_raw + (1 - alpha) * P_{t-1}

Circuit breaker:
- cap delta per cycle: P_t in [P_{t-1} * (1 - c), P_{t-1} * (1 + c)]

### 8.2 Scarcity Score

For item i:
- SS_i = target_stock_i / max(observed_stock_i, 1)
- M_scarcity_i = clamp(1 + k * (SS_i - 1), min_mult, max_mult)

### 8.3 Inflation Monitoring

Basket CPI:
- CPI_t = sum_i (basket_weight_i * price_i_t)

Inflation Index:
- II_t = (CPI_t - CPI_baseline) / CPI_baseline

Money Supply Ratio:
- MSR_t = total_liquid_currency_t / max(active_goods_value_t, 1)

Economic Health Index:
- EHI_t = 100 - (a * abs(II_t) + b * volatility_t + c * inequality_t + d * stockout_rate_t)

### 8.4 Stabilization Policy

If II_t > upper_target and MSR_t > upper_msr:
- fee_multiplier += step_small
- npc_payout_multiplier[oversupplied_categories] -= step_small
- luxury_sink_multiplier += step_small

If II_t < lower_target (deflation risk):
- fee_multiplier -= step_small
- npc_demand_multiplier[bottleneck_categories] += step_small
- upkeep_discount_for_new_players += step_small

All coefficient changes clamped and rate-limited per cycle.

---

## 9. Pseudocode (Implementation-Oriented)

### 9.1 Atomic Trade Execution

```python
async def execute_trade(cmd: BuyListingCmd) -> TradeResult:
    async with uow.transaction() as tx:
        listing = await listings_repo.get_for_update(cmd.listing_id)
        assert listing.status == "open"
        assert listing.quantity_remaining >= cmd.qty

        price = listing.unit_price * cmd.qty
        fees = fee_policy.compute(price, buyer_id=cmd.buyer_id, seller_id=listing.seller_user_id)
        total_buyer_cost = price + fees.buyer_fee + fees.tax

        await balance_repo.debit(cmd.buyer_id, total_buyer_cost)
        await inventory_repo.credit(cmd.buyer_id, listing.item_id, cmd.qty)

        await inventory_repo.debit(listing.seller_user_id, listing.item_id, cmd.qty)
        await balance_repo.credit(listing.seller_user_id, price - fees.seller_fee)

        listing.quantity_remaining -= cmd.qty
        if listing.quantity_remaining == 0:
            listing.status = "sold"

        await ledger_repo.append_many([
            LedgerEntry.trade(...),
            LedgerEntry.fee(...),
            LedgerEntry.tax(...),
        ], idempotency_key=cmd.idempotency_key)

        await outbox_repo.add(DomainEvent.trade_executed(...))

    return TradeResult.success(...)
```

### 9.2 Inflation Monitor Job

```python
async def inflation_monitor_job() -> None:
    snapshot = await economy_service.compute_snapshot()
    policy = stabilization_engine.evaluate(snapshot)

    if policy.requires_action:
        await economy_service.apply_stabilization_cycle(policy)
        await notification_service.broadcast_economy_notice(policy.summary)
```

### 9.3 Contract Fulfillment

```python
async def fulfill_contract(cmd: FulfillContractCmd) -> FulfillmentResult:
    async with uow.transaction():
        contract = await contracts_repo.get_for_update(cmd.contract_id)
        validate_contract_state(contract, cmd.user_id)

        await inventory_repo.consume_bundle(
            user_id=cmd.user_id,
            item_id=contract.item_id,
            qty=contract.quantity_required,
            min_quality=contract.quality_required,
        )

        payout = reward_policy.adjust_for_trust(contract.reward_currency, cmd.user_id)
        await balance_repo.credit(cmd.user_id, payout)

        await reputation_service.grant_from_contract(cmd.user_id, contract)
        await trust_service.update_after_fulfillment(cmd.user_id, contract)

        contract.status = "fulfilled"
        await ledger_repo.append(LedgerEntry.reward(...), idempotency_key=cmd.idempotency_key)
        await outbox_repo.add(DomainEvent.contract_fulfilled(...))

    return FulfillmentResult(payout=payout)
```

  ### 9.4 Benefit Purchase and Discord Role Sync

  ```python
  async def purchase_benefit(cmd: PurchaseBenefitCmd) -> PurchaseResult:
    async with uow.transaction():
      cfg = await guild_config_repo.get_for_update(cmd.guild_id)
      assert cfg.shop_enabled

      offer = await benefit_repo.get_active_for_update(cmd.guild_id, cmd.benefit_id)
      trust = await trust_repo.get(cmd.user_id)
      rep = await reputation_repo.get(cmd.user_id)
      eligibility_policy.assert_can_purchase(offer, trust, rep)

      final_price = pricing_policy.compute_benefit_price(
        base_price=offer.price_currency,
        inflation_index=await economy_repo.get_latest_inflation_index(),
        cfg=cfg,
      )

      await balance_repo.debit(cmd.user_id, final_price)
      order = await shop_order_repo.create(..., price_paid=final_price, idempotency_key=cmd.idempotency_key)
      grant = await benefit_grant_repo.grant_from_offer(order, offer)

      await ledger_repo.append(LedgerEntry.sink(...), idempotency_key=cmd.idempotency_key)
      await outbox_repo.add(DomainEvent.benefit_purchased(grant_id=grant.id))

    await role_sync_service.apply_discord_role(grant.id)
    return PurchaseResult.success(grant_id=grant.id)
  ```

---

## 10. Async and Concurrency Best Practices

1. Use one async session per request/service operation.
2. Use explicit transaction scopes around multi-entity updates.
3. Use SELECT FOR UPDATE for mutable critical rows.
4. Enforce idempotency key uniqueness for interaction retries.
5. Apply optimistic locking/version columns on high-contention aggregates.
6. Use Redis distributed locks for scheduler singleton jobs.
7. Never block event loop with sync I/O; isolate CPU-heavy jobs in worker pool.
8. Backpressure high-frequency commands with Redis token buckets.

---

## 11. Caching Strategy (Redis)

Cache classes:

1. Read-through caches
- item metadata,
- profession definitions,
- active seasonal modifiers.

2. Short-lived market caches (30-120s)
- top listings per item,
- current reference price per item.

3. Player profile caches (15-60s)
- trust/reputation summary,
- housing summary.

Invalidation events:
- TradeExecuted,
- ReputationChanged,
- SeasonalEventChanged,
- ProfessionUpdated.

Cache key examples:
- econ:item_price:{item_id}
- user:trust:{user_id}
- user:rep:{user_id}

---

## 12. Anti-Exploit and Abuse Mitigation

## 12.1 Threats

- Alt-account laundering.
- Price manipulation through self-trading.
- Contract griefing and no-delivery behavior.
- Cancel-relist spam to clutter market.
- Reputation brigading.

### 12.2 Controls

1. Transaction anomaly scoring
- z-score on price deviation,
- suspicious reciprocal trade loops,
- same-cluster account activity.

2. Progressive friction
- escrow requirements for low trust,
- listing caps for suspicious actors,
- temporary trade cool-offs.

3. Ledger audit tools
- immutable append-only ledger,
- admin replay/reconciliation scripts.

4. Reputation integrity
- endorsement weights by reviewer credibility,
- review-decay and anti-burst thresholds.

---

## 13. Scheduler and Background Jobs

Required periodic jobs:

1. market_reprice_job (every 15 min)
- recompute dynamic reference prices.

2. economy_snapshot_job (hourly)
- capture EHI/MSR/II and key aggregates.

3. inflation_monitor_job (hourly)
- evaluate stabilization policy and apply bounded adjustments.

4. seasonal_tick_job (daily)
- rotate seasonal modifiers and activate events.

5. contract_expiry_job (every 10 min)
- fail overdue contracts and update trust scores.

6. benefit_expiry_job (every 10 min)
- expire time-bound perks and remove mapped Discord roles.
- append revocation entries and emit BenefitExpired events.

Job safety:
- distributed lock per job key.
- idempotent handlers.
- retry with jitter and dead-letter logging.

---

## 14. Cogs Organization and Interaction UX

Cogs:
- EconomyCog: balance summary, snapshots, village indicators.
- ProfessionCog: labor actions, crafting, specialization.
- MarketCog: list, buy, cancel, price board.
- BenefitsCog: browse offers, purchase, active benefits, renewal.
- ContractCog: board, accept, fulfill, review.
- HousingCog: home upgrades, décor, hosting.
- SocialCog: vouch, endorsements, trust profile.
- EventsCog: active festivals, emergency responses.

UX rules:
- Every command returns concise summary plus next best action.
- Use paginated embeds and buttons for market browsing.
- Confirm destructive/expensive actions via interaction confirm views.

---

## 15. Observability and Economy Ops

Metrics:
- command latency and error rate,
- trade volume and cancellation rate,
- inflation index, MSR, EHI,
- price volatility by category,
- trust dispute rate,
- suspicious activity count.

Dashboards:
- Economy Health Dashboard,
- Market Risk Dashboard,
- New Player Progress Dashboard.

Alerts:
- II exceeds threshold for N cycles.
- Stockout rate above threshold for key goods.
- anomaly score spikes.

---

## 16. Testing Strategy

Unit tests:
- pricing and inflation formulas,
- trust/reputation scoring,
- taxation and sink policies.

Integration tests:
- end-to-end trade execution transactionality,
- contract fulfillment correctness,
- scheduler stabilization effects.

Property tests:
- no negative balances,
- no item creation without source,
- bounded coefficient movement.

Load tests:
- concurrent listing purchases,
- burst command traffic,
- scheduler overlap resilience.

---

## 17. Deployment and Scaling

1. Single instance MVP
- one bot process,
- one scheduler process,
- Postgres + Redis managed services.

2. Horizontal scale
- multiple bot shards/processes,
- dedicated worker service for heavy background jobs,
- scheduler leader election via Redis lock.

3. Data scaling
- partition market_trades and ledger by time,
- archive historical snapshots to warehouse,
- retain aggregate tables for hot queries.

---

## 18. Security and Compliance

- Principle of least privilege for DB credentials.
- Separate read/write roles.
- Admin action audit trails.
- Signed config and secrets from environment manager.
- GDPR-style deletion path for user data requests (where applicable).

Website-specific controls:
- Discord OAuth2 authorization code flow with PKCE.
- HttpOnly + Secure + SameSite cookies for sessions.
- CSRF protection on state-changing web endpoints.
- Role-based admin authorization for shop configuration routes.
- Rate limiting and bot challenge on login endpoints.

---

## 19. Website Configuration (Main Bot Website)

The bot and website share one domain model and one data layer. The website is not a separate economy authority.

### 19.1 Runtime Topology

- bot process: Discord gateway, cogs, interaction handling.
- api process: REST API for website and optional partner integrations.
- worker process: scheduler and async domain jobs.
- shared Postgres + Redis.

### 19.2 Environment Configuration

Required config groups:

Bot config:
- DISCORD_BOT_TOKEN
- DISCORD_APP_ID
- DISCORD_PUBLIC_KEY

Website config:
- WEB_BASE_URL
- WEB_PORT
- WEB_SESSION_SECRET
- DISCORD_OAUTH_CLIENT_ID
- DISCORD_OAUTH_CLIENT_SECRET
- DISCORD_OAUTH_REDIRECT_URI

Data config:
- DATABASE_URL
- REDIS_URL

Security config:
- SESSION_TTL_SECONDS
- CSRF_SECRET
- ADMIN_ROLE_IDS
- TRUST_PROXY_HEADERS

Shop config defaults:
- SHOP_ENABLED_DEFAULT
- BENEFIT_MAX_ACTIVE_PER_USER
- BENEFIT_MONTHLY_TOP_TIER_CAP
- BENEFIT_INFLATION_LINK_ENABLED
- BENEFIT_TREASURY_SPLIT_BPS

### 19.3 API Surface (Minimum)

Public:
- GET /health
- GET /api/v1/events/active

Authenticated player routes:
- GET /api/v1/me/dashboard
- GET /api/v1/shop/offers
- POST /api/v1/shop/purchase
- GET /api/v1/me/benefits

Admin routes:
- GET /api/v1/admin/shop/config
- PUT /api/v1/admin/shop/config
- POST /api/v1/admin/shop/offers
- PUT /api/v1/admin/shop/offers/{offer_id}
- POST /api/v1/admin/shop/offers/{offer_id}/disable

### 19.4 Website-to-Bot Consistency Rules

- All writes must call application services, never bypass service policies.
- Role assignment/removal must be event-driven and idempotent.
- UI should show pending/failed sync states for Discord role propagation.
- Admin web changes are audited to transactions_ledger metadata and audit logs.

---

## 20. MVP Implementation Roadmap (Engineering)

---

### 20.1 Milestone A: Foundations
- project scaffold,
- DB schema + Alembic,
- repository layer,
- basic cogs shell,
- health checks and logging.

### 20.2 Milestone B: Economy Core
- inventory + crafting,
- market listing/trade,
- transactions ledger,
- pricing engine v1,
- trust/reputation v1.

Website + shop additions in Milestone B:
- Discord OAuth login,
- player dashboard endpoints,
- benefit catalog + purchase flow,
- Discord role sync worker.

### 20.3 Milestone C: Stabilization
- snapshots,
- inflation monitor,
- dynamic sink controls,
- ops dashboards.

### 20.4 Milestone D: Social and Events
- housing v1,
- village projects,
- first seasonal event,
- contract review flows.
- admin web portal for live shop configuration.

Release gates:
- pass concurrency suite,
- pass exploit simulation checks,
- economy dry-run with seeded bots for 2-4 weeks.

---

## 21. Future Technical Extensions

- ML-assisted anomaly detection.
- scenario simulation sandbox for balancing changes before live rollout.
- cross-server trade protocol with federated trust.
- narrative event scripting DSL.
- data science pipeline for economy forecasting.
