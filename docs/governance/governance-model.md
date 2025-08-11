# ICN Governance Model Specification — v0.1

## Overview

The ICN implements **democratic governance** at three scales (local/regional/global) across three domains (economic/civic/technical), ensuring participatory decision-making without plutocracy or corporate capture.

## Core Principles

- **One member, one vote:** Wealth cannot buy additional voting power
- **Subsidiarity:** Decisions made at the lowest effective level
- **Sovereignty:** Organizations retain autonomy over internal affairs  
- **Transparency:** All governance actions are publicly auditable
- **Reversibility:** Governance decisions can be amended (not history)
- **Fork-friendly:** Groups can peacefully exit with their resources

## Governance Scales

### Local (Intra-Organization)

**Scope:** Internal organization decisions

**Mechanisms:**
- **Direct democracy:** All members vote on major decisions
- **Liquid democracy:** Optional delegation to trusted members
- **Working groups:** Specialized teams with delegated authority
- **Sortition:** Random selection for review/audit roles

**Decisions include:**
- Membership admission/removal
- Budget allocation
- Service priorities  
- Internal policies
- Node operator attestations
- Token issuance models

**Process:**
1. **Proposal:** Any member can propose (small CC burn)
2. **Discussion:** Minimum period (e.g., 72 hours)
3. **Vote:** Majority or supermajority as configured
4. **Execution:** Automatic or manual per proposal type
5. **Review:** Post-execution assessment

### Regional (Federation)

**Scope:** Inter-organization coordination

**Representation:**
- Each org sends delegates (elected or rotating)
- Weight = sqrt(membership) with caps to prevent domination
- Decisions prefer consensus, fall back to supermajority

**Decisions include:**
- Settlement rules and exchange rates
- Shared infrastructure investments
- Mutual aid protocols
- Federation membership criteria
- Inter-org dispute resolution
- Regional economic planning

**Process:**
1. **Proposal:** From member org or working group
2. **Circulation:** All member orgs review (1 week minimum)
3. **Assembly:** Delegates meet (physical or virtual)
4. **Consensus attempt:** Facilitated discussion
5. **Vote if needed:** 2/3 supermajority typically
6. **Ratification:** Member orgs can opt-out locally

### Global (Protocol)

**Scope:** Network-wide standards and protocols

**Mechanism:**
- No global governance body
- Changes via multi-federation endorsement
- Fork-friendly for divergent paths

**Decisions include:**
- Protocol version updates
- Event schema changes
- Security parameters
- Network constants
- Emergency responses

**Process:**
1. **RFC:** Proposed in public repository
2. **Implementation:** Reference code provided
3. **Testing:** On devnet with multiple federations
4. **Endorsement:** Federations vote independently
5. **Activation:** When threshold of federations upgrade
6. **Fork option:** Non-adopting federations continue separately

## Decision Types & Requirements

### Constitutional (Fundamental)
- **Scope:** Core principles, membership rights
- **Requirement:** 75% supermajority + 2-week discussion
- **Frequency:** Maximum 2 per year
- **Veto:** 25% can block

### Policy (Operational)
- **Scope:** Rules, procedures, parameters
- **Requirement:** 60% majority + 1-week discussion
- **Frequency:** Monthly cycle typical
- **Override:** 75% can expedite

### Budget (Economic)
- **Scope:** Token issuance, spending, investments
- **Requirement:** 50% majority + published audit
- **Frequency:** Per cycle (weekly/monthly)
- **Constraint:** Cannot exceed treasury + approved minting

### Emergency (Crisis)
- **Scope:** Security, disaster response
- **Requirement:** 3-of-5 emergency committee
- **Duration:** 72 hours maximum
- **Review:** Full vote required for extension

## Governance Actors

### Members
- **Rights:** Propose, discuss, vote, access information
- **Responsibilities:** Participate, stay informed, respect process
- **Requirements:** Verified membership in good standing

### Delegates  
- **Selection:** Elected or appointed by org
- **Mandate:** Bound or free per org rules
- **Rotation:** Regular to prevent entrenchment
- **Recall:** Can be removed by sending org

### Facilitators
- **Selection:** Rotating or elected
- **Role:** Guide discussion, ensure process
- **Powers:** Procedural only, no decision authority
- **Term:** Limited (e.g., 6 months)

### Auditors
- **Selection:** Sortition from member pool
- **Role:** Review decisions and execution
- **Powers:** Report only, no veto
- **Protection:** Cannot be retaliated against

## Democratic Guarantees

### For Cooperatives
- **Worker control:** Workers must own majority stake
- **Patronage voting:** Users/workers vote, not capital
- **Surplus sharing:** Democratic allocation of profits
- **Open books:** Financial transparency required

### For Communities  
- **Open membership:** Cannot exclude based on wealth
- **Equal voice:** One person, one vote regardless of contribution
- **Public process:** All meetings recorded/streamed
- **Minority protection:** Rights cannot be voted away

### For Federations
- **Org sovereignty:** Cannot override internal governance
- **Exit rights:** Orgs can leave with their resources
- **Proportional representation:** Small orgs have voice
- **Diversity requirement:** Must include multiple org types

## Governance Technologies

### Policy Objects
```json
{
  "id": "pol_abc123",
  "type": "budget_allocation",
  "org_id": "org:valley-coop",
  "title": "Q3 Infrastructure Investment",
  "state": "voting",
  "proposer": "did:icn:member789",
  "discussion_start": "2025-08-10T00:00:00Z",
  "voting_start": "2025-08-17T00:00:00Z",
  "voting_end": "2025-08-24T00:00:00Z",
  "threshold": 0.60,
  "current_votes": {"yes": 142, "no": 89, "abstain": 12},
  "execution": "automatic",
  "policy_diff": {...}
}
```

### Liquid Democracy
- Members can delegate voting power by topic
- Delegation chains maximum depth = 3
- Delegates must vote publicly
- Delegation revocable anytime
- Anti-cycling: cannot delegate back

### Sortition Pools
- Random selection from eligible members
- Weighted by participation history
- Cannot serve twice in 6 months
- Compensation for service
- Right to refuse with replacement

### Governance Proofs
Every decision produces verifiable proof:
- Proposal hash + signatures
- Discussion transcript hash
- Vote tallies + member signatures
- Execution receipt
- Audit trail

## Anti-Capture Mechanisms

### Wealth Limits
- Tokens cannot buy votes
- Maximum one membership per org
- Contribution caps on CC delegation
- No plutocratic governance bodies

### Transparency Requirements
- All votes recorded on-chain
- Financial flows public
- Decision rationales published
- Conflicts of interest declared

### Structural Safeguards
- Sortition for sensitive roles
- Mandatory rotation periods
- Supermajority for constitutional changes
- Fork rights guarantee

### Social Defenses  
- Membership vouching networks
- Reputation from participation
- Community review periods
- Cultural norm enforcement

## Dispute Resolution

### Internal (within org)
1. **Mediation:** Peer facilitators attempt resolution
2. **Arbitration:** Internal committee if mediation fails
3. **Vote:** Full membership as last resort
4. **Exit:** Member can leave with portable assets

### Inter-org (within federation)
1. **Direct negotiation:** Org representatives meet
2. **Federation mediation:** Neutral facilitators
3. **Arbitration panel:** Sortition-selected members
4. **Economic remedy:** Settlement via treasury
5. **Relationship remedy:** Adjust exchange rates

### Protocol-level
1. **Technical:** RFC process for bugs/improvements
2. **Economic:** Federation can adjust parameters
3. **Social:** Community pressure and norms
4. **Ultimate:** Fork to new protocol version

## Implementation Requirements

### Technical Infrastructure
- Signed policy objects with versioning
- Immutable audit log
- Vote aggregation with privacy
- Delegation management system
- Sortition randomness source

### Social Infrastructure  
- Facilitation training programs
- Governance documentation
- Decision templates
- Review processes
- Accountability culture

### Economic Infrastructure
- Governance participation incentives
- Facilitation compensation
- Audit rewards
- Delegation rebates
- Emergency reserves

## Success Metrics

- **Participation rate:** > 40% regular, > 60% major decisions
- **Decision time:** < 2 weeks normal, < 72h emergency
- **Representation:** All member orgs active in federation
- **Turnover:** 25-50% annual rotation in roles
- **Satisfaction:** > 70% members approve of process
- **Resilience:** Continues functioning with 30% inactive

## Migration & Evolution

### Onboarding Organizations
1. Demonstrate democratic structure
2. Ratify network principles
3. Implement governance APIs
4. Train facilitators
5. Run parallel for transition
6. Full activation

### Protocol Evolution
1. Small changes: Individual federation experiments
2. Medium changes: Multi-federation coordination
3. Large changes: Formal RFC and reference implementation
4. Breaking changes: Versioned fork with migration path

### Governance Evolution
- Regular review cycles (annual)
- Member-initiated amendments
- Experimental zones for new models
- Learning from federation diversity
- Academic partnership for research
