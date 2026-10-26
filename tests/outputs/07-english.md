These 5 cards describe only the fictional reminder queue in the source.

## Card 1 | New reminders enter pending

- **Core knowledge**: A newly accepted reminder in the source's fictional queue enters the pending state.
- **Brief explanation**: The reminder stays pending until its scheduled time arrives.
- **Example or self-test**: Question: What state does a newly accepted reminder enter? Answer: Pending.

## Card 2 | Selection follows scheduled time

- **Core knowledge**: The source's fictional queue selects ready reminders by scheduled time, earliest first.
- **Brief explanation**: If two scheduled times are equal, the reminder accepted first is selected first.
- **Example or self-test**: Question: How is a tie in scheduled time resolved? Answer: The reminder accepted first is selected first.

## Card 3 | Failed delivery allows one retry

- **Core knowledge**: The source's fictional queue allows one retry after a failed delivery.
- **Brief explanation**: The first failure returns the reminder to pending. A second failure marks it failed and stops automatic delivery attempts.
- **Example or self-test**: Question: What happens after the second delivery failure? Answer: The reminder is marked failed, and automatic delivery attempts stop.

## Card 4 | Cancellation prevents delivery

- **Core knowledge**: A pending reminder in the source's fictional queue can be cancelled to prevent its delivery.
- **Brief explanation**: The cancellation rule in the source applies to pending reminders.
- **Example or self-test**: Question: Which state is explicitly eligible for cancellation? Answer: Pending.

## Card 5 | Completed reminders are not delivered again

- **Core knowledge**: A successful delivery in the source's fictional queue marks the reminder completed.
- **Brief explanation**: Completed reminders are not selected for delivery again.
- **Example or self-test**: Question: Is a completed reminder selected for another delivery? Answer: No.
