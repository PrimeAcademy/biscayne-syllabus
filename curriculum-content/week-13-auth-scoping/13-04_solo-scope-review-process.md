# Scope Review

Instructors need to review student scope documents and provide direction and feedback.

Remember that you are still teaching. Breaking down a large project like this is new to students and they need guidance and reassurange/encouragement. Conside this just a big grading exercise.

Students are expected to do MOST of the work scoping. Reserve writing their features, doing their ERD, or drawing their wireframes as a last resort for those few students who just can't or won't.

**The scope is a Graduation Requirement and MUST be completed to a minimumal degree. If you feel a student is not taking it seriously, tell them so. Be a hard ass. Do not do it for them.**

## Sizing For The Student

In general, you are trying to align each project with the student's individual skill level. 

If a student has struggled in Tier 2, aim at (and be clear you are doing this) Base mode CRUD. Scope maybe 1 stretch goal this week.

If a student is advanced, push them to try new things.

**Be sure you are very aware of the number of NEW things the student will be tackling on this project.** 

- CSS difficulty (varies a ton across students)
- new libraries
- tricky features
- edit mode
- difficult data/queries
- fancy or robust UI (animations, many states, etc)

Everything outside of our core JS/React/Express/DB stuff should be considered NEW. For many students, CSS is pretty NEW.

## Process

Instructors will divide up the students in the cohort. Each instructor will be responsible to track and move their subset of students through the process. Instructors will spot-check or meet with their list of students throughout the week and give feedback on the scope document and ultimately give final approval. This requires dedicated time for support during this week.

> Minimum 2 instructors. The Main instructor can take about 10. The rest needs to be divided up amongst available support instructors.


## Tracking Reviews
Instructors perform scope reviews on their students as needed, providing feedback and clear direction for changes required.

Track approvals in the cohort google spreadsheet. Here are 2 different styles.

- [Mersenne Tracking](https://docs.google.com/spreadsheets/d/1cXrP9jkuC-eXHsWEmH_zNxPHGC8Qrs4dN0PPshTvCCM/edit#gid=1184747342)
- [Solinas Tracking](https://docs.google.com/spreadsheets/d/1cavYmjYpjIIKA8q6A2S2k6Fl4wV0S7m4thIwpnaYR2c/edit#gid=1819929698)


## First Draft Required Sections

**Note: All work must be in the scope document template**

- Application Overview - press these to be short, 3 sentences MAX. Move all feature details to Features.
- Feature Details
    - Each view/page of the application with a wireframe
        - Edit UI is often missed by students, it is required
        - Search bar? Scope it! How does it work - how are Results displayed?
- ERD digram(s) - these usually need lots of attention from instructors
- Technologies - this makes it helpful to spot new libraries and what the CSS plan is


## Scope Review Checklist
This is a student-facing checklist used for peer review, but it's good for instructors, too.

- [Scope Review Checklist](https://github.com/PrimeAcademy/solo-scope-first-draft)

---

## Common Feedback for Common Features
Students will need direction from you around some of these common advanced-ish features.

### 🧑‍🦰 User Profile Picture/Images or File Uploads

Ask the student: How are you going to deal with these images? How does someone pick one, where is the file stored?

> Heroku does not include permanent file hosting. Each time the apps's Dynos are restarted, temporary (non-repo) files are deleted.

Eventually you'll direct them like:

**Base Mode**

Use a URL for either a publicly hosted image or an existing image file in the project repo (like Weekend 10 React Gallery). The URL goes in the database so the user has to enter a URL or chooses a filename for the existing file.

**Stetch**

Actual image or file uploads. This again is broken into 2 stages: basic and full

**Basic**

Files uploaded to local project/host using `multer` or similar. These files are temporary but work fine for demonstration purposes.

**Full**

This is actual cloud storage integration with AWS, Google Cloud/Firebase etc.

---

### 📆 Calenders
Calendars are just a list of things on the DOM organized into a grid. It is very unlikley a student actually needs the crazy UI featues of something like Google Calendar.

**Super Base Verison** 

`input type="date"` gives a pretty good date-picking UI in browsers. Does this solve the problem?

**Base Mode** 

Items/events/things listed on the DOM like we've done for 10 weeks. This is the starting point.

**Stretch** 

Actual calendar/grid layout and UI. There are many libraries to help draw and deal with dates. Google Calendar is Google Events, too, and is probably OVERKILL for this project's needed functionality.
