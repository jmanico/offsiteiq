# REQUIREMENTS.md: Company Trip Planner

Version: 0.1 (initial draft from stakeholder interview, 2026-10-01)
Status: DRAFT. Items marked `OPEN` require stakeholder confirmation before implementation.

## 1. Purpose

A business application that plans company trips for distributed teams. Given a destination, a date range, and budgets, the system identifies travel and lodging options for every participating employee, proposes local venues for meetings and entertainment, and produces a coordinated itinerary for the trip.

## 2. Scope

### 2.1 In scope
- Trip definition (destination, dates, budgets)
- Employee roster and home locations as trip inputs
- Flight and hotel availability and pricing via third-party provider integrations
- Discovery of local meeting spaces and entertainment venues at the destination
- Itinerary generation and coordination across all participants

### 2.2 Out of scope (v1, OPEN)
- Booking and payment execution (the interview only specified "see what's available, what the rates are")
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

Each requirement is written to be verifiable by an automated test or an explicit manual check.

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

## 5. Security Requirements

Aligned to OWASP ASVS 5.0. These are mandatory, not advisory.

### 5.1 Authentication and session management

| ID | Requirement | ASVS ref |
|----|-------------|----------|
| SEC-AUTH-01 | Authentication SHALL be delegated to the company identity provider via OIDC. No local password store. | V6 |
| SEC-AUTH-02 | Authorization code flow with PKCE SHALL be used. Implicit flow is prohibited. Follow RFC 9700. | V10 |
| SEC-AUTH-03 | Session tokens SHALL be stored in HttpOnly, Secure, SameSite=Lax (or Strict) cookies. | V7 |
| SEC-AUTH-04 | Sessions SHALL expire after an idle timeout and an absolute timeout. Values `OPEN`. | V7 |

### 5.2 Authorization

| ID | Requirement | ASVS ref |
|----|-------------|----------|
| SEC-AUTHZ-01 | The system SHALL enforce role-based access with at least two roles: Organizer and Participant. | V8 |
| SEC-AUTHZ-02 | Only an Organizer of a Trip SHALL view budgets, aggregate costs, and other Participants' travel details. | V8 |
| SEC-AUTHZ-03 | A Participant SHALL view only their own travel details and shared events. | V8 |
| SEC-AUTHZ-04 | Every data access SHALL be authorized server-side by Trip membership and role. Client-side checks are not sufficient. | V8 |
| SEC-AUTHZ-05 | Direct object references (trip IDs, participant IDs) SHALL be unguessable (UUIDv4 or equivalent) and access-checked on every request. | V8 |

### 5.3 Input validation and output encoding

| ID | Requirement | ASVS ref |
|----|-------------|----------|
| SEC-INPUT-01 | All inputs (user and Provider responses) SHALL be validated against an explicit schema before use. Provider responses are untrusted. | V2 |
| SEC-INPUT-02 | All output to HTML, JSON, logs, and exports SHALL be contextually encoded. | V3 |
| SEC-INPUT-03 | Database access SHALL use parameterized queries exclusively. | V2 |
| SEC-INPUT-04 | Location and venue data returned from Providers SHALL be treated as untrusted content and never rendered as HTML. | V3 |

### 5.4 Data protection and privacy

| ID | Requirement | ASVS ref |
|----|-------------|----------|
| SEC-DATA-01 | Employee home locations and travel details are personal data. Collection SHALL be limited to what the trip search requires (see FR-PART-05). | V14 |
| SEC-DATA-02 | Personal data SHALL be encrypted in transit (TLS 1.2 minimum, TLS 1.3 preferred) and at rest. | V9, V11 |
| SEC-DATA-03 | Trip data SHALL have a defined retention period after Trip end date, after which it is deleted. Period `OPEN`. | V14 |
| SEC-DATA-04 | Logs SHALL NOT contain personal data, budgets, or Provider credentials. | V16 |
| SEC-DATA-05 | `OPEN`: confirm whether any Participants are in jurisdictions with specific data residency requirements. | V14 |

### 5.5 Third-party integrations and secrets

| ID | Requirement | ASVS ref |
|----|-------------|----------|
| SEC-INTEG-01 | Provider API credentials SHALL be stored in a secrets manager, never in source, config files, or environment files committed to version control. | V11 |
| SEC-INTEG-02 | Each Provider integration SHALL use a dedicated credential with the minimum scope the Provider supports. | V11 |
| SEC-INTEG-03 | Outbound Provider calls SHALL have timeouts, retry limits, and circuit breakers so one failing Provider cannot block search. | V13 |
| SEC-INTEG-04 | Provider responses SHALL be size-limited and schema-validated before parsing. | V2, V13 |
| SEC-INTEG-05 | All third-party libraries SHALL be reviewed for CVE history and maintenance status before adoption, with transitive dependencies analyzed. A software bill of materials SHALL be generated per build. | V15 |

### 5.6 Logging and monitoring

| ID | Requirement | ASVS ref |
|----|-------------|----------|
| SEC-LOG-01 | Authentication events, authorization failures, budget overrides, and Itinerary changes SHALL be logged with actor, timestamp, and affected Trip ID. | V16 |
| SEC-LOG-02 | Logs SHALL be write-once from the application's perspective. | V16 |

## 6. Non-Functional Requirements

| ID | Requirement | Verification |
|----|-------------|--------------|
| NFR-PERF-01 | A travel search for a Trip of up to 50 Participants SHALL return results within 30 seconds, with Provider calls executed concurrently. `OPEN`: confirm Participant count ceiling. | Load test. |
| NFR-AVAIL-01 | Search SHALL degrade gracefully: if a Provider fails, results from remaining Providers are returned with the failure noted. | Integration test. |
| NFR-TEST-01 | Automated test coverage SHALL be at least 80% of lines, enforced in CI. | CI gate. |
| NFR-TEST-02 | All Provider integrations SHALL have mocked contract tests that run without network access. | CI gate. |
| NFR-SEC-01 | CI SHALL run SAST and dependency scanning on every pull request, and block merge on high or critical findings. | CI gate. |
| NFR-REVIEW-01 | All AI-generated code SHALL pass the test gate and receive human review before merge. | Process check. |

## 7. Data Model (initial)

```
Employee
  id: UUID
  display_name: string
  email: string (from IdP)
  home_location: Location

Location
  city: string
  region: string
  country: ISO 3166-1 alpha-2
  lat: decimal
  lon: decimal
  nearest_airport: IATA code (optional)

Trip
  id: UUID
  organizer_id: UUID (Employee)
  destination: Location
  start_date: date
  end_date: date
  trip_budget: Money
  per_person_budget: Money
  status: enum {draft, searching, planned, archived}
  created_at, updated_at: timestamp (UTC)

Money
  amount: decimal(12,2)
  currency: ISO 4217

TripParticipant
  trip_id: UUID
  employee_id: UUID
  selected_flight: TravelOption (optional)
  selected_lodging: LodgingOption (optional)

TravelOption / LodgingOption
  provider: string
  provider_ref: string
  price: Money
  normalized_price: Money
  exchange_rate_used: decimal (optional)
  fetched_at: timestamp (UTC)
  details: JSON (schema-validated per Provider)

Venue
  id: UUID
  trip_id: UUID
  type: enum {meeting, entertainment}
  name: string
  location: Location
  capacity: integer
  indicative_cost: Money (optional)

ItineraryItem
  id: UUID
  trip_id: UUID
  type: enum {flight, lodging, meeting, event}
  starts_at, ends_at: timestamp (UTC)
  tz: IANA time zone
  participants: [UUID]  (empty means all)
  ref: UUID (TravelOption, LodgingOption, or Venue)
  version: integer
```

## 8. Open Questions

Numbered for tracking. Each must be resolved or explicitly deferred before the item it blocks is implemented.

1. Does the system book and pay, or only search and compare? (Blocks scope of 4.3.)
2. Which Providers for flights and hotels in v1: direct airline APIs, an aggregator, or both? (Blocks FR-SRCH-09.)
3. Which data source for local venues? (Blocks FR-VENUE-04.)
4. Are all employees Participants by default, or does the Organizer select them? (Blocks FR-PART-02.)
5. Where does the employee roster come from: HRIS integration, CSV import, or manual entry? (Blocks FR-PART-04.)
6. Can the Organizer override budget violations, and who approves? (Blocks FR-TRIP-05.)
7. Itinerary export format. (Blocks FR-ITIN-05.)
8. Data retention period after Trip end. (Blocks SEC-DATA-03.)
9. Maximum expected Participant count per Trip. (Blocks NFR-PERF-01.)
10. Any data residency constraints for Participants' locations. (Blocks SEC-DATA-05.)
11. Session timeout values. (Blocks SEC-AUTH-04.)
12. Acceptable staleness for cached Provider pricing. (Blocks FR-SRCH-08.)

## 9. Traceability to interview

| Interview statement | Requirements |
|---------------------|--------------|
| Company trip planner for business | Section 1, 2 |
| Inputs: location, date range, whole-trip budget, individual budget | FR-TRIP-01 through FR-TRIP-06 |
| Need all workers and their locations as inputs | FR-PART-01 through FR-PART-05 |
| Connects to many airlines and hotels for availability and rates | FR-SRCH-01 through FR-SRCH-09, SEC-INTEG-01 through SEC-INTEG-05 |
| Location for local entertainment and meeting spots | FR-VENUE-01 through FR-VENUE-04 |
| Coordination of local itinerary for this meeting | FR-ITIN-01 through FR-ITIN-06 |
