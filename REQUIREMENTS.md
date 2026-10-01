# REQUIREMENTS.md: Company Trip Planner

Version: 0.1 (initial draft from stakeholder interview, 2026-10-01)
Status: DRAFT. Items marked `OPEN` require stakeholder confirmation before implementation. Security requirements (`SEC-*`, `NFR-SEC-*`) live in [SECURITY.md](SECURITY.md) §3; the data model lives in [ARCHITECTURE.md](ARCHITECTURE.md) §5.

## 1. Purpose

A business application that plans company trips for distributed teams. Given a destination, a date range, and budgets, the system identifies travel and lodging options for every participating employee, proposes local venues for meetings and entertainment, and produces a coordinated itinerary for the trip.

## 2. Scope

### 2.1 In scope
- Trip definition (destination, dates, budgets)
- Employee roster and home locations as trip inputs
- Flight and hotel availability and pricing via third-party provider integrations
- Discovery of local meeting spaces and entertainment venues at the destination
- Itinerary generation and coordination across all participants

### 2.2 Out of scope (v1)
- Booking and payment execution. v1 searches and compares only (stakeholder decision, 2026-10-01).

The following are also out of scope for v1, pending confirmation (`OPEN`):
- Ground transportation, visas, travel insurance
- Expense reimbursement and accounting integration

## 3. Definitions

| Term | Definition |
|------|------------|
| Trip | A single planned company event with one destination and one date range |
| Participant | An employee included in a Trip |
| Organizer | A user who creates and manages a Trip |
| Trip Budget | Total allowed spend for the whole Trip |
| Per-Person Budget | Maximum allowed spend attributable to one Participant |
| Provider | An external airline, hotel, or venue data source |
| Itinerary | The time-ordered schedule of travel, meetings, and events for a Trip |

## 4. Functional Requirements

### 4.1 Trip inputs

| ID | Requirement | Verification |
|----|-------------|--------------|
| FR-TRIP-01 | The system SHALL accept a destination as a structured location (city, region, country, and resolved geocoordinates), not free text alone. | Unit test: free text is resolved to a canonical location or rejected with an error. |
| FR-TRIP-02 | The system SHALL accept a date range with an explicit start date and end date, where end date >= start date and start date >= today. | Unit test on boundary conditions. |
| FR-TRIP-03 | The system SHALL accept a Trip Budget as a decimal amount with an ISO 4217 currency code. | Unit test: amount without currency is rejected. |
| FR-TRIP-04 | The system SHALL accept a Per-Person Budget as a decimal amount with an ISO 4217 currency code. | Unit test. |
| FR-TRIP-05 | The system SHALL reject a Trip where (Per-Person Budget x Participant count) exceeds the Trip Budget, unless the Organizer explicitly overrides with a recorded justification. | Unit test. `OPEN`: confirm override is allowed. |
| FR-TRIP-06 | All monetary values SHALL be stored and computed as fixed-point decimals, never binary floating point. | Code review and unit test. |

### 4.2 Participants

| ID | Requirement | Verification |
|----|-------------|--------------|
| FR-PART-01 | The system SHALL maintain a roster of employees with, at minimum, a unique employee identifier, display name, and home location (structured, geocoded). | Schema test. |
| FR-PART-02 | The Organizer SHALL be able to select which employees participate in a Trip. The default selection is `OPEN` (interview said "all workers"; confirm whether all are included by default). | Integration test. |
| FR-PART-03 | Each Participant's home location SHALL be used as the origin for travel search. | Integration test: search requests carry the correct origin per Participant. |
| FR-PART-04 | The roster SHALL be importable from an authoritative source. `OPEN`: HRIS, CSV, or manual entry. | Integration test once source is chosen. |
| FR-PART-05 | Home location SHALL be stored at the minimum precision needed for travel search (nearest airport or city), not a street address, unless a documented business need exists. | Schema review. |

### 4.3 Travel and lodging search

| ID | Requirement | Verification |
|----|-------------|--------------|
| FR-SRCH-01 | For each Participant, the system SHALL query one or more flight Providers for options from the Participant's origin to the destination within the Trip date range. | Integration test with mocked Providers. |
| FR-SRCH-02 | The system SHALL query one or more hotel Providers for availability at the destination covering the full date range. | Integration test with mocked Providers. |
| FR-SRCH-03 | Each returned option SHALL include price, currency, Provider name, and a Provider reference identifier. | Contract test. |
| FR-SRCH-04 | The system SHALL normalize all returned prices to the Trip Budget currency using a dated exchange rate, and SHALL record the rate used. | Unit test. |
| FR-SRCH-05 | The system SHALL flag any option whose cost would cause the Participant to exceed the Per-Person Budget. | Unit test. |
| FR-SRCH-06 | The system SHALL compute the running Trip total (all Participants' selected flights plus lodging) and flag when it exceeds the Trip Budget. | Unit test. |
| FR-SRCH-07 | Provider integrations SHALL be implemented behind a common internal interface so Providers can be added or removed without changing search logic. | Architecture review; at least two Providers per category behind one interface. |
| FR-SRCH-08 | Provider results SHALL be cached with a recorded fetch timestamp and SHALL display that timestamp to the user. | Unit test. `OPEN`: acceptable staleness window. |
| FR-SRCH-09 | Provider list for v1 is `OPEN`. The interview said "many airlines and hotels"; confirm whether this means direct airline APIs or an aggregator (GDS or similar). | N/A |

### 4.4 Local venues

| ID | Requirement | Verification |
|----|-------------|--------------|
| FR-VENUE-01 | The system SHALL return candidate meeting spaces at the destination with capacity, address, and indicative cost. | Integration test with mocked Provider. |
| FR-VENUE-02 | The system SHALL return candidate entertainment venues (restaurants, activities) at the destination. | Integration test with mocked Provider. |
| FR-VENUE-03 | Venue results SHALL be filterable by capacity >= Participant count. | Unit test. |
| FR-VENUE-04 | Venue data source for v1 is `OPEN`. | N/A |

### 4.5 Itinerary

| ID | Requirement | Verification |
|----|-------------|--------------|
| FR-ITIN-01 | The system SHALL produce one Itinerary per Trip containing all selected flights, lodging, meeting sessions, and entertainment events in chronological order. | Unit test. |
| FR-ITIN-02 | The Itinerary SHALL produce a per-Participant view showing only that Participant's travel plus shared events. | Unit test. |
| FR-ITIN-03 | The system SHALL detect scheduling conflicts (overlapping events, events scheduled before a Participant's arrival or after departure) and report them. | Unit test. |
| FR-ITIN-04 | All times SHALL be stored in UTC with the originating IANA time zone recorded, and displayed in the destination's local time zone. | Unit test. |
| FR-ITIN-05 | The Itinerary SHALL be exportable. Format is `OPEN` (ICS, PDF, or both). | Integration test once format is chosen. |
| FR-ITIN-06 | Changes to the Itinerary SHALL be versioned, recording who changed what and when. | Integration test. |

## 5. Non-Functional Requirements

| ID | Requirement | Verification |
|----|-------------|--------------|
| NFR-PERF-01 | A travel search for a Trip of up to 50 Participants SHALL return results within 30 seconds, with Provider calls executed concurrently. `OPEN`: confirm Participant count ceiling. | Load test. |
| NFR-AVAIL-01 | Search SHALL degrade gracefully: if a Provider fails, results from remaining Providers are returned with the failure noted. | Integration test. |
| NFR-TEST-01 | Automated test coverage SHALL be at least 80% of lines, enforced in CI. | CI gate. |
| NFR-TEST-02 | All Provider integrations SHALL have mocked contract tests that run without network access. | CI gate. |
| NFR-REVIEW-01 | All AI-generated code SHALL pass the test gate and receive human review before merge. | Process check. |

## 6. Open Questions

Numbered for tracking. Each must be resolved or explicitly deferred before the item it blocks is implemented.

1. ~~Does the system book and pay, or only search and compare?~~ Resolved 2026-10-01: search and compare only (§2.2).
2. Which Providers for flights and hotels in v1: direct airline APIs, an aggregator, or both? (Blocks FR-SRCH-09.)
3. Which data source for local venues? (Blocks FR-VENUE-04.)
4. Are all employees Participants by default, or does the Organizer select them? (Blocks FR-PART-02.)
5. Where does the employee roster come from: HRIS integration, CSV import, or manual entry? (Blocks FR-PART-04.)
6. Can the Organizer override budget violations, and who approves? (Blocks FR-TRIP-05.)
7. Itinerary export format. (Blocks FR-ITIN-05.)
8. Data retention period after Trip end. (Blocks SEC-DATA-03 in SECURITY.md.)
9. Maximum expected Participant count per Trip. (Blocks NFR-PERF-01.)
10. Any data residency constraints for Participants' locations. (Blocks SEC-DATA-05 in SECURITY.md.)
11. Session timeout values. (Blocks SEC-AUTH-04 in SECURITY.md.)
12. Acceptable staleness for cached Provider pricing. (Blocks FR-SRCH-08.)

## 7. Traceability to interview

| Interview statement | Requirements |
|---------------------|--------------|
| Company trip planner for business | Section 1, 2 |
| Inputs: location, date range, whole-trip budget, individual budget | FR-TRIP-01 through FR-TRIP-06 |
| Need all workers and their locations as inputs | FR-PART-01 through FR-PART-05 |
| Connects to many airlines and hotels for availability and rates | FR-SRCH-01 through FR-SRCH-09, SEC-INTEG-01 through SEC-INTEG-05 |
| Location for local entertainment and meeting spots | FR-VENUE-01 through FR-VENUE-04 |
| Coordination of local itinerary for this meeting | FR-ITIN-01 through FR-ITIN-06 |
