# Election Audit


## Overview of Project

Tom is a Colorado Board of Elections employee who is assigned by his manger to perform an election audit of the tabulated results for U.S. Congressional precinct in Colorado. Three primary voting methods are taken into account: Mail-in ballots, punch cards, and direct recording electronic or DRE counting machines. Altogether the votes cast by these three methods will determine the final election results. As a part of the audit to find the election winner, the following metrics were calculated: 

1.Total Number of Votes cast for the election

2.Total Number of Votes for each Candidate

3.The percentage of Votes for each Candidate

4.The winner of the election based on the popular vote

5.The voter turnout for each county

6.The percentage of votes for each county out of the total count

7.The county with the highest turnout


### Purpose

The purpose of the project was to assist Tom in the audit of tabulated results for a US congressional precinct in Colorado. Using the data source of a CSV file containing the tabulated totals for the elections from various voting methods, the objective was to automate the process by writing a python script to successfully audit the election. Also, if this audit is done successfully with Python, the code will be used for senatorial districts and local elections.
## Technologies
#### Project is created with:

1. ```Python 3.7.6```
2. ```Visual Studio Code 1.49.3``` 

#### Data Source: ```election_results.csv```

## Election Audit Results

The analysis of the election audit shows that:
- There were 369,711 votes cast in the congressional election.

- Breakdown of the number of votes and the percentage of total votes for each county in the precinct:
     - Jefferson county had 10.5% of the vote and 38,855 number of votes
     - Denver county had 82.8% of the vote and 306,055 number of votes
     - Arapahoe county had 6.7% of the vote and 24,801 number of votes

- Denver county had the largest number of votes.

- Breakdown of the number of votes and the percentage of the total votes each candidate:
     - Charles Casper Stockham received 23.0% of the vote and 85,213 number of votes
     -  Diana DeGette received 73.8% of the vote and 272,892 number of votes
     -  Raymon Anthony Doane received 3.1% of the vote and 11,606 number of votes

-  The winner of the election was:
     -  Diana DeGette, who received 73.8% of the vote and 272,892 number of votes


**Election Results Screenshots**


**Output to terminal**

![Election_results.png](https://github.com/smj452/Election_Analysis/blob/master/Resources/election_results.png)





**Output to text file**

![Election_Analysis_txt](https://github.com/smj452/Election_Analysis/blob/master/Resources/Election_analysis_txt.png)



**Python script**

```python

# Add our dependencies.
import csv
import os

# Add a variable to load a file from a path.
file_to_load = os.path.join("Resources/election_results.csv")
# Add a variable to save the file to a path.
file_to_save = os.path.join("election_analysis.txt")

# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county = []
county_votes={}


# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

# 2: Track the largest county and county voter turnout.
largest_county = ""
largest_voter_turnout = 0


# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1 # candidate_Votes[candidates_name] = candiadte_votes[candidates_name] +1

        # 4a: Write a decision statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county:      

            # 4b: Add the existing county to the list of counties.
            county.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name]=0


        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1 


# Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a repetition statement to get the county from the county dictionary.
    for c in county:
          
        # 6b: Retrieve the county vote count.
        county_vote = county_votes[c]
        # 6c: Calculate the percent of total votes for the county.
        county_vote_percent = 100*float(county_vote)/float(total_votes)

        # 6d: Print the county results to the terminal.
        results = f"{c}: {county_vote_percent:.1f}% ({county_vote:,})\n"
        print(results)
         # 6e: Save the county votes to a text file.
        txt_file.write(results)
         # 6f: Write a decision statement to determine the winning county and get its vote count.
        if largest_voter_turnout < county_vote:
            largest_voter_turnout = county_vote
            largest_county = c

    # 7: Print the county with the largest turnout to the terminal.
    print("-------------------------\n")
    print("Largest County Turnout: ", largest_county)
    print(f"-------------------------\n")

    # 8: Save the county with the largest turnout to a text file.
    txt_file.write("-------------------------\n")
    txt_file.write(f"Largest County Turnout: {largest_county}\n")
    txt_file.write("-------------------------\n")

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)

```


## Election-Audit Summary

This congressional election audit was completed successfully with Python. The audit would usually be done in Excel, but we were able to automate the process using Python. By automating the process, we were able to generate the election audit report quicker and with less errors. With a few modifications, the code written to complete this audit can be used for any election, including senatorial districts and local elections.

The script can be modified and used to analyze any elections. Following are two examples for modifying the script:

1. Change the file_to_load(the file that contains election data).
Two ways to change the file are:
 - Change the current file name with the new file name that requires analysis.
 - Add python input function that will allow the user to enter the file name and then run an analysis on the input file.

2.Update the script and add another 'for' loop. Here are two scenarios that will require the script update to add another "for' loop:
 - Perform analysis for senatorial districts and local elections
 - Perform analysis for presidential elections










