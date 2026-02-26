### Order Statuses

Below is a description of order statuses orders may encounter.

| Value | User Action? | Description |
| ----- | ----------------------------------------------------------- | ------------------ |
| Draft | X | Order has been drafted on your end, but not yet submitted to RegHub. At this stage the order can be edited, deleted, or submitted. The user action in this state is to complete the draft and submit the order. |
| Pending |   | Order has been submitted to RegHub for processing and it is awaiting results. |
| Complete | X | Order has been completed and you can now poll for any documents or data. The user action in this state is to retreive and/or view the results of your order. |
| Invalid | X | Order was not able to be completed at a government registry and has been set invalid with appropriate message indicating why. Normally only applies to PPSA registration order types. The user action here is to either edit the order to correct some information, or cancel your order. |
| Exception |   | Order is going to be reviewed by RegHub and then will be completed, cancelled, or set invalid depending on the order type. __No action required on your end.__ |
| AwaitingHighVolumeResults |   | Order has been placed at the government registry but will take longer than normal SLAs to get the result from them. __No action required on your part__ as it will change status to Complete once information is received from the government registry. |
| Cancelled | X | Order was unable to be completed at the government registry or an attestor cancelled a UBO/did not complete a UBO before the link expired. Cancelled is a type of Complete status as this order cannot be resubmitted. The only user action here is to potentially create a new order depending on why the original order was cancelled. |
| OutOfRegistryHours |   | The government registry in question has closed for the day/weekend. Your order will be held by RegHub in priority sequence and completed once the government registry opens and resumes service. |
| AwaitingExternalInformation |   | A UBO report has been sent to the attestor but has not yet been completed by them. This will change to Complete once they finish it or Cancelled if they do not action it prior to the invitation expiring. |
