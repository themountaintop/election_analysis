# Election Analysis

## Purpose

The purpose of this project was to create a way to automatically tally votes, show how many votes each candidate received, show the percentage of the total votes the candidates received, and to show which candidate won the election. This info, along with which county had the largest turnout and the amount they had, would then be displayed in the election_analysis.txt file located in the "analysis" folder.

## Election Info

* Total number of votes cast in the congressional election:
  * 369,711

* County with the largest turnout:
  * Denver County
  * Total Turnout: 306,055

* Breakdown of candidates, percentage of votes received, and total number of votes received:  
  * Charles Casper Stockham: 23.0% (85,213)
  * Diana DeGette: 73.8% (272,892)
  * Raymon Anthony Doane: 3.1% (11,606)

* Election results
  * Winner: Diana DeGette
  * Vote Count: 272,892
  * Percentage of Total: 73.8%
  * Election Audit Summary

#### Script used to obtain, calculate, and output to a file the information above:

```
# Add our dependencies.
import csv
import os

# Add a variable to load a file from a path.
file_to_load = os.path.join("Resources", "election_results.csv")
# Add a variable to save the file to a path.
file_to_save = os.path.join("analysis", "election_analysis.txt")

# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county_list = []
county_votes = {}

# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

# 2: Track the largest county and county voter turnout.
largest_county_turnout = ""
largest_county_turnout_count = 0
largest_county_percentage = 0

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
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_list:

            # 4b: Add the existing county to the list of counties.
            county_list.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

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

    # 6a: Write a for loop to get the county from the county dictionary.
    for county in county_list:
        # 6b: Retrieve the county vote count.
        county_vote = county_votes.get(county)
        # 6c: Calculate the percentage of votes for the county.
        county_vote_percentage = float(county_vote)/float(total_votes) * 100
        # 6d: Print the county results to the terminal.
        county_results = (f"{county}: {county_vote_percentage:.1f}% ({county_vote:,})\n")
        print(county_results)
        # 6e: Save the county votes to a text file.
        txt_file.write(county_results)
        # 6f: Write an if statement to determine the winning county and get its vote count.
        if (county_vote > largest_county_turnout_count) and (county_vote_percentage > largest_county_percentage):
            largest_county_turnout_count = county_vote
            largest_county_percentage = county_vote_percentage
            largest_county_turnout = county

    # 7: Print the county with the largest turnout to the terminal.
    winning_county_summary = (
        f"-------------------------\n"
        f"County with the largest turnout: {largest_county_turnout}\n"
        f"Turnout: {largest_county_turnout_count}\n"
        f"-------------------------\n"
        )
    print(winning_county_summary)

    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(winning_county_summary)

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

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
  
## Election Audit Summary

This script calculated all the required information to declare the winner of a specific election provided the data file. In order to use this script to scale for any election, some suggestions are as follows:

* Create input box or search box for the user(s) to indicate where they would like to pull the election data from.
#### Example code (sourced from https://stackoverflow.com/questions/20865010/how-do-i-create-an-input-box-with-python):
```
from tkinter import *

master = Tk()
e = Entry(master)
e.pack()

e.focus_set()

def callback():
    print e.get() # This is the text you may want to use later

b = Button(master, text = "OK", width = 10, command = callback)
b.pack()

mainloop()
```
#### Example Output:

![Screen Shot 2022-09-14 at 11 15 04 AM](https://user-images.githubusercontent.com/58227052/190194615-0b711ac3-fa22-4e5a-a121-589ccd721ce8.png)

* Take the input from the user as file path to tell the script where to look for the file that is needed for the analysis:
#### Example code (sourced from https://stackoverflow.com/questions/52343574/how-to-convert-users-input-to-file-path-in-python)

```
import os

path = input("enter filepath: ")

for f in os.listdir(os.path.expanduser(path)):
    print(f)

```

Using these two ideas together, you could create an input box asking for a file path of where the file resides in the computer. This is just one small way of adding to this script in order to scale this use for other elections. 
