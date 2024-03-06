# Data-Analyst-Internship-Submission
Transform current employee data from a columnar format into a historical, row-based versioning format suitable for database storage using Python.
import csv
import pandas as pd
from datetime import datetime

# Sample CSV data
data = '''Employee Code,Manager Employee Code,Date of Joining,Date of Exit,Compensation,Compensation 1,Compensation 1 date,Compensation 2,Compensation 2 date,Review 1,Review 1 date,Review 2,Review 2 date,Engagement 1,Engagement 1 date,Engagement 2,Engagement 2 date
1,,2021-01-01,,20000,,,,,,,,,,,,
2,1,2021-01-01,,20000,10000,2022-01-01,20000,2023-01-01,9,2021-06-01,9.5,2022-06-01,4,2021-03-01,5,2022-03-01
3,1,2021-01-01,2023-12-31,20000,10000,2022-01-01,20000,2023-01-01,9,2021-06-01,9.5,2022-06-01,4,2021-03-01,5,2022-03-01'''

# Parse CSV data
rows = list(csv.DictReader(data.splitlines()))

# Initialize a list to store historical records
historical_records = []

# Iterate through the rows and create historical records
for row in rows:
    # Process dates
    doj = datetime.strptime(row['Date of Joining'], '%Y-%m-%d')
    if 'Date of Exit' in row:
        doe = datetime.strptime(row['Date of Exit'], '%Y-%m-%d')
    else:
        doe = None

    # Process compensation
    if 'Compensation 1' in row:
        compensation_history = [
            {'effective_date': doj, 'amount': int(row['Compensation'])},
            {'effective_date': datetime.strptime(row['Compensation 1 date'], '%Y-%m-%d'), 'amount': int(row['Compensation 1'])},
        ]
        if 'Compensation 2' in row:
            compensation_history.append({
                'effective_date': datetime.strptime(row['Compensation 2 date'], '%Y-%m-%d'),
                'amount': int(row['Compensation 2'])
            })
    else:
        compensation_history = [{'effective_date': doj, 'amount': int(row['Compensation'])}]

    # Process reviews
    review_history = []
    if 'Review 1' in row:
        review_history = [
            {'effective_date': datetime.strptime(row['Review 1 date'], '%Y-%m-%d'), 'score': float(row['Review 1'])}
        ]
        if 'Review 2' in row:
            review_history.append({
                'effective_date': datetime.strptime(row['Review 2 date'], '%Y-%m-%d'),
                'score': float(row['Review 2'])
            })

    # Process engagements
    engagement_history = []
    if 'Engagement 1' in row:
        engagement_history = [
            {'effective_date': datetime.strptime(row['Engagement 1 date'], '%Y-%m-%d'), 'score': int(row['Engagement 1'])}
        ]
        if 'Engagement
