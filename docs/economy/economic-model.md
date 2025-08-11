# ICN Economic Model Specification — v0.1

## Overview

The ICN implements a **three-layer economy** operating at **three scales**, creating a 3×3 matrix of economic activity that enables cooperative economics without corporate capture.

### Three Economic Layers
1. **Infrastructure Layer (CC/mana):** Non-transferable credits that fuel network operations
2. **Value Layer (tokens):** Transferable units representing real goods, services, and labor  
3. **Social Layer (governance):** Democratic decision-making that shapes economic activity

### Three Operational Scales
1. **Local (intra-org):** Direct member-to-member exchange within organizations
2. **Regional (federation):** Inter-organization coordination and settlement
3. **Global (protocol):** Network-wide standards and async federation peering

## Core Principles

- **No anonymous participation:** All actors must have organizational membership
- **Democratic requirement:** All organizations must demonstrate democratic governance
- **Dual capacity:** Both cooperatives and communities can issue tokens, run infrastructure, and manage budgets
- **Specialization not hierarchy:** Cooperatives focus on economic production, communities on civic coordination, but capabilities overlap
- **Capital boundaries:** Tokens cannot purchase voting rights or governance influence
- **Equal membership, unequal contribution:** One person one vote, but compensation reflects value created (see [Value Theory](value-theory.md))

## Organizational Types

### Cooperatives (Economic Specialization)
- **Primary focus:** Production of goods/services
- **Examples:** Worker co-op, platform co-op, housing co-op, credit union
- **Capabilities:** Issue tokens, run nodes, create budgets, participate in governance
- **Requirements:** Democratic ownership, member economic participation

### Communities (Civic Specialization)  
- **Primary focus:** Geographic/social coordination and mutual aid
- **Examples:** Neighborhood assembly, municipality, bioregion, interest group
- **Capabilities:** Issue tokens, run nodes, create budgets, provide services
- **Requirements:** Democratic participation, open membership in defined scope

### Federations (Coordination Layer)
- **Composition:** 2+ organizations (any mix of cooperatives/communities)
- **Purpose:** Inter-org settlement, shared infrastructure, mutual aid, standards
- **Cannot:** Override member org sovereignty or rewrite local history
- **Must:** Include at least one cooperative AND one community for full network participation

## Contribution Credits (CC) Economy

### Generation
- **Source:** Only from running network infrastructure (compute, storage, bandwidth, witnessing)
- **Formula:** `cc_earned = resource_contribution × trust_weight × scarcity_index`
- **Split:** Node operators share with endorsing org (default 80/20, gov-configurable)

### Distribution
- **Delegation:** Orgs delegate CC allowances to members for baseline access
- **Baseline:** Every member gets minimum CC for governance participation
- **Stacking:** Multi-membership allows multiple delegations (within caps)
- **Decay:** Unused CC decays over time; regen rate decays with inactivity

### Consumption
- **Every transaction:** Burns minimum CC as anti-spam
- **Compute jobs:** Burns CC proportional to resources
- **Governance:** Minimal burn for votes and proposals
- **Priority:** Additional CC burn for faster processing

### Constraints
- **Non-transferable:** CC cannot be sold or transferred between identities
- **Capped accumulation:** Maximum CC balance = 5-7× daily regen rate
- **No purchasing:** Cannot buy CC with tokens (must contribute infrastructure)

## Token Economy

### Issuance Models
Both cooperatives and communities can issue tokens via:
- **Labor vouchers:** Tokens for hours worked or services provided
- **Production shares:** Tokens representing goods produced
- **Budget allocations:** Democratic approval for project/service funding
- **Basic income:** Optional universal distribution to members
- **Mutual credit:** Member-to-member credit creation (with limits)

### Token Flows

**Intra-organization:**
```
Member A ←→ Member B
   ↓          ↑
  Org Treasury
   (fees/taxes)
```

**Federation Levy (Cooperative → Community):**
```
Cooperative Revenue
    ↓
10-20% Levy (democratic)
    ↓
Federation Pool
    ↓
60% Communities | 20% Infrastructure | 20% Reserves
```

**Inter-organization (same federation):**
```
Org A → Invoice → Federation Clearing → Settlement → Org B
                        ↓
                 Exchange rates,
                 Netting, Demurrage
```

**Cross-federation:**
```
Federation X ← Async proof exchange → Federation Y
     ↓                                      ↑
Exchange rates via                    No direct settlement
market makers                         (convert at edges)
```

### Economic Controls
- **Demurrage:** Optional decay (1-5% monthly) to discourage hoarding
- **Transaction fees:** Orgs can charge 0.1-2% on internal transfers
- **Exchange rates:** Federations set inter-org rates via governance
- **Capital controls:** Limits on cross-federation token flows
- **Wealth caps:** Optional maximum token holdings per identity

## Individual Participation

### Membership Requirements
- **Primary membership:** Must have one "home" organization
- **Identity anchor:** Home org provides root attestation
- **Dual membership typical:** Most join both a cooperative (economic) and community (civic)
- **Multi-membership:** Can join multiple orgs with additive benefits
- **Migration:** Can change home org with departure/arrival attestations

### Node Operation
1. **Individual requests** node operator attestation from member org
2. **Org reviews** technical capability and resources
3. **Attestation issued** with conditions (uptime SLA, revenue split)
4. **Node earns CC** with org trust weight backing
5. **Revenue split** between operator and org (e.g., 80/20)
6. **Bad behavior** → org revokes → node loses trust weight

### Wallet Model
- **Identity (DID):** Permanent, self-sovereign, portable
- **Attestations:** Portable proofs of membership/roles/reputation
- **Token balance:** Fully portable between organizations
- **CC delegation:** Tied to active org memberships
- **Migration:** Preserves history while updating home org

## Transaction Mechanics

Every economic action has two costs:
1. **CC cost:** Infrastructure fuel (anti-spam, resource compensation)
2. **Token cost:** Economic value (price of goods/services)

### Example Flows

**Purchase from co-op:**
- Pay: 50 tokens (value) + 0.1 CC (network fee)
- Co-op receives: 49.5 tokens (after 1% org fee)
- Network receives: 0.1 CC (burned)

**Submit compute job:**
- Pay: 10 CC (resource cost) + optional token tip
- Executor receives: 8 CC regen rate increase
- Executor's org receives: 2 CC regen rate increase

**Community service:**
- Volunteer: 4 hours at community garden
- Receive: 20 tokens from community budget
- Burn: 0.01 CC (record attestation)

## Economic Governance

### Budget Cycles
- **Period:** Typically weekly or monthly
- **Process:** Proposal → discussion → vote → allocation
- **Constraints:** Cannot exceed treasury + approved minting
- **Review:** Mandatory post-cycle assessment

### Monetary Policy (per org/federation)
Governed parameters include:
- Token issuance caps and models
- Demurrage rates (0-5% monthly)
- Transaction fees (0.1-2%)
- CC delegation amounts
- Node revenue splits
- Exchange rate bands

### Anti-Capture Mechanisms
- **Democratic proof:** Orgs must demonstrate one-member-one-vote
- **Wealth limits:** Token holdings can't affect voting weight
- **Sortition pools:** Random selection for sensitive roles
- **Transparency:** All economic flows publicly auditable
- **Fork-friendly:** Orgs can leave federations if captured

## Success Metrics

- **Velocity:** Token circulation rate > 2× monthly
- **Distribution:** Gini coefficient < 0.4 within orgs
- **Participation:** > 60% members economically active monthly
- **Infrastructure:** < 20% CC generation concentration
- **Settlement:** > 99% inter-org settlements clear on schedule
- **Resilience:** Network operates with 30% nodes offline

## Migration Path

### From Capitalism
1. Organization incorporates democratically
2. Joins/forms local community for civic foundation
3. Issues tokens for internal economy
4. Runs nodes for CC generation
5. Connects to federation for inter-org trade
6. Gradually reduces external currency dependence

### Between Organizations
1. Member initiates migration request
2. Current org signs departure attestation
3. New org validates identity and history
4. New org issues arrival attestation
5. Wallet migrates with full token balance
6. CC delegation updates to new org
7. Reputation carries forward with time decay
