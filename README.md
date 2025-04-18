# Email Marketing Campaign 

## Dataset Description

This dataset contains information from an email marketing campaign and is composed of **three relational tables**:

### 1. `email_table.csv`
Provides metadata about each email sent during the campaign.

| Column Name           | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `email_id`            | Unique identifier for each email sent.                                      |
| `email_text`          | Describes whether the email had a "short" or "long" body text.              |
| `email_version`       | Indicates if the email was "personalized" (with user's name) or "generic".  |
| `hour`                | The local hour when the email was sent.                                     |
| `weekday`             | The day of the week the email was sent.                                     |
| `user_country`        | Country of the user receiving the email (based on IP address).              |
| `user_past_purchases` | Number of past purchases by the user.                                       |

### 2. `email_opened_table.csv`
Tracks which emails were opened by users (clicked and presumably read).

| Column Name | Description                     |
|-------------|---------------------------------|
| `email_id`  | ID of the email that was opened |

### 3. `link_clicked_table.csv`
Logs emails where the internal link was clicked by the user.

| Column Name | Description                              |
|-------------|------------------------------------------|
| `email_id`  | ID of the email whose link was clicked   |

