# TigerApps Course Update Guide
Once per semester, as soon as possible following the release of the next semester's courses, **TigerSnatch**, **ReCal**, and **PrincetonCourses** must be updated to allow students to plan their courses. Follow the steps below to update each of these three apps.

# TigerSnatch
## Update course data
1. TigerSnatch will automatically update to the new term within 15 minutes of new courses being released.
    * **Verify** that TigerSnatch has updated to the new term by going to https://tigersnatch.com/dashboard and checking that the new term is shown in the status indicator (upper left), and checking that searching for a course works.

## Add new notifications intervals (if necessary)
1. After TigerSnatch exits maintenance mode, head back to the Admin Tools table. Locate the second row, labeled "Modify Notifications Schedule".
2. Click the yellow "G-Sheet" button. This Google Sheet controls when TigerSnatch sends notifications for open spots.
    * You must be given access to this Google Sheet. To gain access, email nicholaspad@gmail.com. Note that only the TigerApps chair and/or lead developers will be granted access.
3. **Carefully** read the instructions in the sheet titled "README".
4. Switch to the sheet titled "Data" and enter new datetime intervals (if already added, verify that they are correct) for each course enrollment period and the add/drop period for the next semester.
    * Make sure to add a comment in the far-right column for each datetime interval entry.
5. Wait 10 minutes. TigerSnatch will poll this sheet automatically during that time.
    * **Verify** that the new datetime intervals have registered by hovering over the "status" badge in the upper left of the TigerSnatch website. The last sentence should begin with "Next notifications period:".

Contact nicholaspad@gmail.com and shannon.heh@princeton.edu if you need help or if you have questions about any of these steps.

# ReCal
## Update course data
1. Update wording on the main page, update list of active term codes, and set the current term code.
    * **Example commit**: https://github.com/PrincetonUSG/recal/pull/11/commits/5e855b2605e4c05deadfc0d3b4555418d1ffce51
    * In `settings/common.py`, remove the leftmost term code and append the new term code to `ACTIVE_TERMS`. `ACTIVE_TERMS` should contain the **3** latest term codes, including the new term code, and `CURR_TERM` should be set to the new term code.
2. Open and merge the pull request after it is approved.
3. Wait 24 hours. ReCal will perform a course update in the background during that time.
    * **Verify** that ReCal has updated to the new term by going to https://recal.io and checking that the new term has been added to the tab bar, and checking that searching for a course works.

Contact nicholaspad@gmail.com, shannon.heh@princeton.edu, and ca9@princeton.edu if you need help or if you have questions about any of these steps.

# PrincetonCourses
## Update course data
1. PrincetonCourses will automatically update to the new term within 24 hours of new courses being released.
    * **Verify** that PrincetonCourses has updated to the new term by going to https://princetoncourses.com and checking that the new term has been added to the "Semester" dropdown, and checking that searching for a course works.

## Update course evaluations
### One-time setup
1. Create a file named `.env` in the PrincetonCourses source code root directory and enter the following key-values pair (with quotes but without angled brackets):
    * `MONGODB_URI='<PrincetonCourses production MongoDB connection string>'`
    * Obtain this URI from the Config Vars section in the PrincetonCourses production web panel in Heroku (go to the Settings tab).
2. Define the following environment variables through the Terminal (e.g. using `$ conda env config vars set <key>=<val>`):
    * `CONSUMER_KEY=<MobileApp API consumer key>`
    * `CONSUMER_SECRET=<MobileApp API consumer secret>`
    * Obtain these values from the Config Vars section in the PrincetonCourses production web panel in Heroku (go to the Settings tab).
3. Install the Python `requests` library:
```
$ pip install requests
```
### Perform update
1. Execute the below command in the PrincetonCourses source code root directory.
```
$ node importers/scrapeEvaluations.js
```
2. Follow the instructions printed by the script. Specifically, you'll need to retrieve a cookie value that allows for CAS authentication. After this, when prompted for a MongoDB query, just press enter (which tells the script to scrape all courses).
3. Let the script complete (takes about 30 minutes).
    * **Verify** that new evaluations were scraped by searching for a course (e.g. COS126) on PrincetonCourses and checking that evaluations from the previous term were added.

Contact nicholaspad@gmail.com, shannon.heh@princeton.edu, and ca9@princeton.edu if you need help or if you have questions about any of these steps.

# Contacts
* **Nicholas Padmanabhan** - nicholaspad@gmail.com
* **Shannon Heh** - shannon.heh@princeton.edu
* **Charles An** - ca9@princeton.edu
