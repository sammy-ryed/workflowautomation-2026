# Workflow Automation 2026 🔧

Three BPMN 2.0 process models, built and validated in Camunda Modeler. Employee leave, online purchase orders, and IT support tickets — each one mapped end to end with proper gateways and no dead-end paths.

## What's in here

```
scenario1-leave-approval.bpmn
scenario2-purchase-order.bpmn
scenario3-it-service-request.bpmn
```

Every file is standalone. Open any one of them in Camunda Modeler and you'll get the full diagram, no missing dependencies.

---

## 1. Employee Leave Approval

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

## 2. Online Purchase Order Processing

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

## 3. IT Service Request

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

## Opening these in Camunda Modeler

1. Grab Camunda Modeler (free) from [camunda.com/download/modeler](https://camunda.com/download/modeler/).
2. Drag any `.bpmn` file straight onto the app window, or use **File → Open File**.
3. The diagram renders fully — shapes, labels, gateway logic, all of it. Edit freely from there.

If a file opens as read-only, right-click it in Windows Explorer → **Properties** → uncheck **Read-only**, then reopen.

## Validation notes

Each model was checked for:
- Every task and gateway has matching incoming/outgoing sequence flows.
- Every gateway branch is accounted for — no path that just stops.
- Every path terminates at a real end event.
- Valid, well-formed BPMN 2.0 XML (parses clean, no malformed tags).

## Repo structure

Flat and simple on purpose. No subfolders, no build step, no dependencies. Clone it, open the files, done.

```bash
git clone https://github.com/sammy-ryed/workflowautomation-2026.git
```