# ENG-008 – Notifications Before Automation

Today's lab wasn't about smart plugs.

It was about proving communication.

Before automating anything important, the notification pipeline must be trusted.

The first attempt failed because the generic notification service was selected instead of the device-specific mobile notification service.

The correction demonstrated an important engineering principle:

Verify interfaces before troubleshooting implementations.

Another observation was that Home Assistant reported an automation timeout while still successfully creating the automation.

Rather than assuming failure, validation through testing confirmed successful operation.

This reinforced another principle:

Evidence beats assumptions.
