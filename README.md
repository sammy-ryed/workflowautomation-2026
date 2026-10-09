# Workflow Automation 2026

BPMN and DMN models for workflow automation coursework. The original experiments contain three core BPMN workflows and two BPMN workflows paired with DMN decision tables. The new assignment models the Student Project Approval & Allocation System, with a report, eight diagram views and a register of 26 failure paths.

## What's in here

```
exp 1/
  scenario1-leave-approval.bpmn
  scenario2-purchase-order.bpmn
  scenario3-it-service-request.bpmn

exp 2/
  exercise1-loan-risk-routing.bpmn
  exercise1-evaluate-loan-risk.dmn
  exercise2-order-discount-fulfillment.bpmn
  exercise2-calculate-discounts.dmn

exp new assignment/
  Assignment_1_Complete.bpmn
  Assignment_1_Diagrams.pdf
  Assignment_1_Report.pdf
  Failure_Path_Register.csv
  *_Diagram.svg
  Diagram-*.png
  README.txt
  Validation_Summary.txt
```

No subfolders inside each experiment, no build step, no dependencies. Clone it, open the files, done.

---

## exp 1 — Core Workflows

### 1. Employee Leave Approval

An employee submits a leave request through the HR system. The system checks their balance before it ever reaches a human.

**Flow:**
1. Employee submits the request → start event.
2. HR system checks leave balance.
3. **Enough balance?**
   - No → insufficient-balance notification sent → done.
   - Yes → request routed to the manager.
4. **Manager approves?**
   - Yes → balance updates, approval notification goes out → done.
   - No → rejection notification goes out → done.

Three possible endings, one gateway pair, zero ambiguity about who gets notified when.

### 2. Online Purchase Order Processing

Standard e-commerce path from "customer clicks buy" to "package is on its way," with two early exits built in.

**Flow:**
1. Customer places an order → start event.
2. System checks product availability.
3. **In stock?**
   - No → customer gets an out-of-stock notice → process ends here.
   - Yes → payment gets processed.
4. **Payment successful?**
   - No → customer gets a payment-failure notice → process ends here.
   - Yes → order confirmed, product prepped for shipment.
5. Order ships, customer gets a shipping confirmation → done.

Two ways this can end early, one way it ends well. All three are real end events, not just abandoned branches.

### 3. IT Service Request

An employee reports a problem. The help desk triages by severity, a technician takes it from there, and it either gets fixed in-house or kicked out to an external vendor.

**Flow:**
1. Employee reports the issue → start event.
2. Help desk registers the request and checks severity.
3. **Severity level?**
   - Low → assigned to a support technician.
   - High → assigned to a senior technician.
4. Technician investigates (both severity paths merge here).
5. **Resolvable internally?**
   - Yes → technician fixes it.
   - No → escalated to the external service provider.
6. Both resolution paths merge, help desk updates the ticket status.
7. Employee gets notified the issue is resolved → done.

Two gateways, two merge points, one clean exit.

---

## exp 2 — Decision-Driven Processes

### 1. Loan Risk Routing (BPMN + DMN)

A loan application comes in. The process hands off to a DMN decision table that evaluates risk based on credit score, debt-to-income ratio, and loan amount. The result routes the application to the right review path.

**BPMN Flow:**
1. Application received → start event.
2. Call DMN decision table to evaluate risk.
3. **Risk level?**
   - Low → auto-approve, notify applicant → done.
   - Medium → route to loan officer for manual review.
   - High → route to senior underwriter with additional checks.
4. Reviewer decides → approve or reject → notify applicant → done.

**DMN Table (exercise1-evaluate-loan-risk.dmn):**
- Inputs: creditScore, debtToIncomeRatio, loanAmount
- Output: riskLevel (Low, Medium, High)
- Rules cover the full combination space, no gaps.

### 2. Order Discount Fulfillment (BPMN + DMN)

An order comes in. A DMN decision table calculates the discount based on customer tier, order volume, and promotional code. The process then applies the discount and fulfills the order.

**BPMN Flow:**
1. Order received → start event.
2. Call DMN decision table to calculate discount.
3. Apply discount to order total.
4. Check inventory and process payment.
5. **Payment successful?**
   - No → notify customer of failure → done.
   - Yes → confirm order, prepare shipment → done.

**DMN Table (exercise2-calculate-discounts.dmn):**
- Inputs: customerTier, orderVolume, promoCode
- Output: discountPercentage
- Rules handle tier stacking, volume thresholds, and promo overrides.

---

## exp new assignment - Student Project Approval & Allocation

**Assignment 1 only.** A student team submits a proposal, the department validates and screens it, a committee reviews it, and an available faculty guide formally accepts the team. Success requires an authoritative allocation record, archived evidence and delivered student notification.

The model covers correction and revision loops, rejection and lapse, deadlines and review escalation, guide shortages and refusals, technical and notification failures, and student or guide withdrawal. All five responsibility lanes are represented. Reservation compensation and idempotent cleanup release guide capacity when an allocation fails or is withdrawn.

### Files and suggested reading order

1. [Report](exp%20new%20assignment/Assignment_1_Report.pdf): happy path, proposed policies, task types, gateway rationale and the full failure register.
2. [Diagram PDF](exp%20new%20assignment/Assignment_1_Diagrams.pdf): eight views covering the overall case, approval workflow and detailed stages.
3. [Editable BPMN model](exp%20new%20assignment/Assignment_1_Complete.bpmn): BPMN 2.0 XML with eight diagram planes. Validation and screening are reusable called processes; the other stages use subprocesses.
4. [Failure Path Register](exp%20new%20assignment/Failure_Path_Register.csv): 26 failure categories mapped to exact BPMN element IDs, owners, recovery paths and limits.
5. SVG and PNG previews: `*_Diagram.svg` and `Diagram-1.png` through `Diagram-8.png` provide independent views when an editor cannot navigate every diagram plane.
6. [Validation summary](exp%20new%20assignment/Validation_Summary.txt) and [local README](exp%20new%20assignment/README.txt): checks, limitations and opening instructions.

### Validation and assumptions

The assignment XML passes the official OMG BPMN 2.0 XSD, unique-ID and reference checks, lane-membership checks, per-diagram reachability checks and exclusive/parallel gateway structure checks. All failure detection IDs in the register exist in the model. The report and diagram PDFs were rendered and visually reviewed.

This is a descriptive model (`isExecutable=false`), not a deployed workflow or an engine-tested application. Deadlines, retry counts and department policy details are explicitly labelled modelling assumptions. Called processes share case data and counters under the report's modelling contract; engine-specific variable mappings are not configured. Post-completion guide replacement is a separate process.

Only extracted assignment files are stored here; the transfer ZIP is not included.

---

## Opening these in Camunda Modeler

1. Grab Camunda Modeler (free) from camunda.com/download/modeler.
2. Drag any .bpmn or .dmn file straight onto the app window, or use File → Open File.
3. Use the editor to inspect and edit the original experiment diagrams. For the new assignment, use subprocess and called-process navigation where supported; the supplied PDF, SVG and PNG files show all eight views.

If a file opens as read-only, right-click it in Windows Explorer → Properties → uncheck Read-only, then reopen.

## Validation notes for exp 1 and exp 2

Each model was checked for:
- Every task and gateway has matching incoming/outgoing sequence flows.
- Every gateway branch is accounted for — no path that just stops.
- Every path terminates at a real end event.
- DMN tables have complete rule coverage, no overlapping or missing rules.
- Valid, well-formed BPMN 2.0 and DMN 1.3 XML (parses clean, no malformed tags).

## Repo structure

Three flat coursework folders: `exp 1`, `exp 2` and `exp new assignment`. No build step is required to view the supplied models and documents.

```bash
git clone https://github.com/sammy-ryed/workflowautomation-2026.git
```