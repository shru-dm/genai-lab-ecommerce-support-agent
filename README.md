# genai-lab-ecommerce-support-agent
Ecommerce support agent


E-Commerce Checkout Event Severity Analyzer Overview This Zapier Agent monitors and analyzes cart events from an e-commerce website, evaluating them against baseline metrics to identify checkout issues that require immediate action. The agent automatically classifies events by severity (HIGH/LOW) and routes them for alerting or logging.

**What It Does:** Monitors cart events from Google Sheets in real-time Evaluates severity based on checkout step, cart value, failure patterns, and baseline risk rates Triggers alerts for high-severity events and logs low-severity events Maintains state to ensure no events are processed twice Executes webhooks to notify downstream systems Key Features ✅ Stateful processing with timestamp tracking ✅ Intelligent severity classification ✅ Baseline metric comparison ✅ Mobile failure detection ✅ Webhook integration for real-time notifications

**How It Works:**  Decision Rules Events are marked as HIGH severity if ANY condition is met:

Checkout step = PAYMENT Cart value ≥ $500 Baseline rate indicates elevated risk for the current event Multiple MOBILE failures in the current batch Action Mapping HIGH severity → ALERT (requires immediate attention) LOW severity → LOG (informational only) Processing Flow Read State Retrieves last_processed_timestamp from State Sheet Filter Events Loads cart_events where timestamp > last_processed_timestamp Evaluate Applies severity rules to each eligible event Write Output Creates one row per event in Agent Sheet with: command_id (unique identifier) action (ALERT or LOG) severity (HIGH or LOW) reason (concise explanation) Checkout details (step, cart value, device, baseline rate, timestamp) Update State Sets last_processed_timestamp to the maximum timestamp processed Data Requirements 

**Input:** Cart Events Sheet Required columns:
timestamp (ISO format) checkout_step (CART, CHECKOUT, PAYMENT, etc.) cart_value (numeric) device (MOBILE, DESKTOP, TABLET) baseline_rate (numeric risk indicator) 

**Output:** Agent Sheet Automatically populated with:
command_id, action, severity, reason checkout_step, cart_value, device, baseline_rate, timestamp

**State: State Sheet Maintains:**
key: "last_processed_time" value: ISO timestamp of last processed event Trigger On Demand - Runs when manually invoked by the user

Integration Points Google Sheets: Reads cart events, manages state, writes decisions 
Webhook: Sends high-severity alerts downstream. Running the Agent Ensure cart events are updated in Google Sheets. Click "Run" in Zapier Agents Agent processes new events and updates both sheets automatically. Webhooks fire for HIGH severity events. Error Handling If a required value is missing from cart_events, processing stops for that row. State is only updated after all rows are successfully processed. No duplicate processing due to timestamp-based filtering. Use Cases 🛒 E-commerce checkout failure monitoring 📊 Cart abandonment analysis 🚨 Payment failure alerting 📈 Mobile checkout performance tracking
