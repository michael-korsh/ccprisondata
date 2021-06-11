# ccprisondata

This Python script retrieves the data points relevant to [Injustice Watch’s Cook County Jail COVID-19 tracker](https://datastudio.google.com/u/3/reporting/1AI4THiXJ_6Nt-9NXwE0MfO_DUaa1Koxi/page/hcyJB) each day, and updates the Google Sheet containing the data.

Each part of the scraper (PrisonDataScraper.ipynb) is sectioned off into segments for each task. Below is an explanation of each task the scraper executes:

## Installing and Importing Relevant Libraries
Installs libraries (Java, tabula, pygsheets) and imports packages needed for future steps. Also authenticates user information to allow edit access to Injustice Watch's internal spreadsheet.

## Cook County Jail Population — Retrieval
Every day, the [Cook County Sheriff's office](https://www.cookcountysheriff.org/data/) publishes a new PDF file with the jail population and electronic monitoring population for that day. The PDFs for each day have a similar file-path name structure, so this function constructs the file-path for a day’s PDF using a given date and uses tabula to scrape for the relevant data.

Returns **jail_data,** a dictionary with:
* PDF Path: The link to the PDF file for that day, to allow for verification that the program linked to the correct file.
* Date (corresponding with the PDF file).
* Jail Population: The number of prisoners currently housed inside the jail.
* Electronic Monitoring: The number of prisoners currently under electronic monitoring.

## Cook County Jail COVID-19 Cases — Retrieval
Every day, the Cook County Sheriff's office updates [their webpage](https://www.cookcountysheriff.org/covid/covid-19-cases-at-ccdoc/) with COVID-19 statistics as of 5 p.m. the day before. Because all that is changed on the webpage is the numbers, this function uses BeautifulSoup to scrape the HTML file for the website and retrieve the relevant statistics from each of the sentences on the page.

Returns **covid_data,** a dictionary with:
* Date: The date corresponding to when the data was last updated.
* Current Positives: The number of detainees who have currently tested positive for COVID-19. 
* Recovered: The number of detainees who have recovered from COVID-19. 
* Negative: The number of detainees who have tested negative for COVID-19. 
* Detainee Deaths: The number of detainees who have died from COVID-19 in the facility.
* Sheriff's Staff Positives: The total number of Sheriff's Office staff (officers + other employees) who have currently tested positive for COVID-19.
* Officer Positives: The number of correctional officers who have currently tested positive for COVID-19.
* Recovered Sheriff's Employees: The number of Sheriff's employees who have recovered for COVID-19.
* Correctional Officer Deaths: The number of correctional officers who have died from COVID-19.

## Helper Functions
* **get_today_jail_data** runs the get_jail_data function for today’s date, so you don’t need to manually enter the date every time you want to update the spreadsheet.
* **reformat_date** modifies a date in the proper format so it can be used later.

## Initialize Today’s Data
Creates 2 variables to store the data for today: one for the jail data and one for the COVID-19 data. The program will pass those values to subsequent functions.

## Updating Spreadsheets
Uses pygsheets to update the jail population and COVID-19 case spreadsheets with the retrieved data.

### Update Jail Spreadsheet
* Checks to see if jail data is available for the given day. If no data is available, it will indicate this and stop running, making no changes to the spreadsheet.
* Checks to see if the day's jail data is already in the spreadsheet. If so, it will indicate this and stop running, making no changes to the spreadsheet.
* If new jail data is available and not already in the spreadsheet, the program will write a new row in the Google Sheet with the data for that day.

### Update COVID Spreadsheet
* Checks to see if the latest COVID-19 data is already in the spreadsheet. If so, it will indicate this and stop running, making no changes to the spreadsheet.
* If new data is available and not already in the spreadsheet, the program will write a new row in the Google Sheet with the data for that day.

## Daily Operations
Like "Initialize Today's Data," daily_operations runs both of the update spreadsheet functions with one command.

## RUNNING THE PROGRAM
Executes the functions. Divided into 2 sub-sections:
* **Mondays:** Allows user to enter in weekend dates, and will update the jail population spreadsheet with data from the weekend.
* **Daily Operations:** Run every day.

# Created by Michael Korsh for [Injustice Watch,](https://www.injusticewatch.org) Spring 2021.

