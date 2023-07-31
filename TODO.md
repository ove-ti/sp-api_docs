# TODO List for Amazon SP API Fulfillment Integration

## Current Workflow:
- When a shipment (delivery) is made, the fulfillment information is sent to EDI.
- From EDI, the information is sent to ShipStation.
- ShipStation then sends the fulfillment information to Amazon.

## Proposed Workflow:
- Send the fulfillment information directly to Amazon using the SP API (either in JSON or XML format).

## Tasks:

1. **Obtain the JSON/XML model for fulfillment.**
    - Research and document the structure of the JSON/XML model that Amazon's SP API expects for fulfillment information.

2. **Convert fulfillment information to JSON/XML format.**
    - Develop a process or script to convert the existing fulfillment information into the required JSON/XML format.

3. **Obtain necessary credentials.**
    - Obtain and securely store any necessary API keys, tokens, or other credentials required for the SP API.

4. **Run tests.**
    - Test the new process with non-production data to ensure it works correctly.
    - Resolve any issues or errors that arise during testing.
    - Once testing is successful, implement the new process in the production environment.
