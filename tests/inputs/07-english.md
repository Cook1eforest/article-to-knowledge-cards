# Understanding a fictional reminder queue

This is a fictional teaching system, not documentation for an existing product.

## Pending state

A newly accepted reminder enters the pending state until its scheduled time arrives.

## Selection order

Ready reminders are selected in order of scheduled time, earliest first. When two scheduled times are equal, the reminder accepted first is selected first.

## Failed delivery

A failed delivery returns the reminder to pending for one retry. A second failure marks it failed and stops automatic delivery attempts.

## Cancellation

A pending reminder can be cancelled. Cancellation prevents delivery of that reminder.

## Completion

A successful delivery marks the reminder completed. Completed reminders are not selected for delivery again.
