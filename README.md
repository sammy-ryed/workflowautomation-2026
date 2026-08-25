# Workflow Automation 2026

Seven process and decision models, built and validated in Camunda Modeler. Three BPMN processes for core workflows, two more BPMN processes paired with DMN decision tables for business rules. Every file opens cleanly on its own.

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

## Opening these in Camunda Modeler

1. Grab Camunda Modeler (free) from camunda.com/download/modeler.
2. Drag any .bpmn or .dmn file straight onto the app window, or use File → Open File.
3. The diagram renders fully — shapes, labels, gateway logic, decision tables, all of it. Edit freely from there.

If a file opens as read-only, right-click it in Windows Explorer → Properties → uncheck Read-only, then reopen.

## Validation notes

Each model was checked for:
- Every task and gateway has matching incoming/outgoing sequence flows.
- Every gateway branch is accounted for — no path that just stops.
- Every path terminates at a real end event.
- DMN tables have complete rule coverage, no overlapping or missing rules.
- Valid, well-formed BPMN 2.0 and DMN 1.3 XML (parses clean, no malformed tags).

## Repo structure

Flat and simple on purpose. Two experiment folders, no build step, no dependencies.

```bash
git clone https://github.com/sammy-ryed/workflowautomation-2026.git
```