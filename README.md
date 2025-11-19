The YouTrack's Advanced Search and Data Extrapolation
Introduction:

A work made by interns Adriano Rueda & Nicola Pucci from ITS Prodigi to bring value to the team.

Index
Advanced Search
1.1 Definition
1.2 How to activate it
1.3 Syntax
1.4 Search Suggestions
1.5 Wildcards in Attribute-based Search
1.6 Single Value Search
1.7 Searching for a Range of values
1.8 Searching in Attributes that Store Text
1.9 Query Completion
1.10 Visual Indicators

Data Extrapolation

1. Advanced Search
1.1 Definition
The YouTrack's Advanced search is the second option for searching issues in the platform. It allows you to search issues using attribute and value pairs.

1.2 How to activate it
To activate advanced search you have to select it in the settings button alt text to the right of the search box.

1.3 Syntax
To compose an attribute-based search query there are various ways. Below that with examples:

: to specify the value of an attribute
project: IDEADEV
# finds all issues in the project with name IDEADEV
# to find issues with that value or state
#Open
# finds all issues with state Open
- to exclude a value
 project: -IDEADEV
 # finds all issues where project name is *NOT* IDEADEV
.. to find issues between two values
project: IDEADEV Fixed in builds: 41238 .. 43006
# finds all issues in the project with name IDEADEV that were fixed in builds from 41238 to 43006
{} to enclose values that contain spaces with braces
Type: {Support Request}
# finds all issues that are assigned the Support Request type
and to include one or more issues to the original one
project: IDEADEV State: -Fixed commenter: John Davis Type: {Support Request}
# finds support requests in the IDEADEV project that include comments from John Davis and are not assigned the Fixed state
or to include an alternative issue
project: IDEADEV and (Type: {Support Request} or commenter: John Davis)
# finds issues in the IDEADEV project that are either support requests or have a comment by John Davis
1.4 Search suggestions
Search suggestion can be made Smart o for User Accounts.

Smart:
The values suggested for subsequent attributes are restricted to the sets of values used in these projects are well when you search for a value only used in a specific set of projects. Un example occurs when you search for items that are assigned the State: On hold.

User Accounts: Search attributes that reference users display usernames in the auto-completion list. These suggestions are only offered when you are granted the Read User Basic permissions in at least one project.

1.5 Wildcards in Attribute-based Search
You can use the * wildcards to find values in custom fields that store an enum, a state, an ownedField, a version, a build or a string. You can also use that for sprint names, project names and tags. Wildcards are not supported for fields that store a user, a date, a period, a float or an integer. The ? wildcard is not supported in attribute-based search queries even for custom fields that store a string.

1.6 Single Value Search
YouTrack allows to search for single values replacing a number sign or a minus in front of the value to put.
Some example:

#{In Progress} -{Support Request} finds all issues that are in progress that are not support requests;
Pricing: {-100} .. {-500} finds issues with pricing values between -100 and -500;
#me -Resolved finds issues that you reported, assigned or commented to that are unresolved.
1.7 Searching for a Range of Values
Like YouTrack allows you to search for single values allows you also to search for a Range of Values.
To search for a range:

Enter the name of the attribute followed by a :;
Enter the value that defines the upper or lower bound of the range followed by ..;
Enter the value that closes the range. use the * as a wildcard.
Execute the search query.
Some Query of Search for a Range of Values with closed like Range:

Spent time: 1h .. 2h returns issues in wich amount of spent time is at least one hour but not greater than two hours.
Created: 2018-03-10 .. 2018-03-13 returns issues created from March 10 to March 13, 2018.
Actual start: 2025-03-25T09:00 .. 2025-03-25T18:00 returns issues started on March 25, 2025 between 09:00 and 18:00.
Story Points: 1 .. 5 returns issues that are assigned at least one story point but not more than five story points.
Price: {-100} .. {-500} returns issues that have a price value between -100 and -500.
Type: Bug #Resolved Fixed in builds: 41238 .. 43006 returns resolved issues that are categorized as bugs that are fixed in builds from 41238 to 43006.
Now with open like Range:

Spent time: 8h1m .. * returns issues in which the amount of spent time is greater than one day.
Created: 2018-03-10 .. 2018-03-13 returns issues created on or before March 10, 2018.
Actual start: 2025-03-25T014:00 .. * returns issues started on or after March 25, 2025 at 14:00.
Story Points: 10 .. * returns issues that are assigned 10 story points and higher.
Price: {-500} .. * returns issues have a price value of -500 or higher.
votes: 30 .. * #Unresolved returns unresolved issues that have 30 or more votes.
Priority: * .. Major #Unresolved returns unresolved issues that are assigned major priority or higher.
Type: Bug #Resolved Fixed in builds: 41238 .. * returns resolved issues that are categorized as bugs that are fixed in builds 41238 and higher.
1.8 Searching in Attributes that Store Text
Attributes like the issue summary, description or comments are stored as text. When you specify one of these attributes in your search query YouTrack understands that you want to check the specified attribute for one or more words. Consequentially YouTrack searches inside the attribute for matches to the specified text. The syntax between text search and attribute-based search is slightly different. In this kind of search you can enter as many words as you want, in any order. See it with some queries:

project: IDEADEV commenter: John Davis description: customer support returns all issues in the IDEADEV project that include comments from John Davis and contain forms of the words "customer" and "support" in the description.
project: IDEADEV commenter: John Davis description: "customer support" returns all issues in the IDEADEV project that include comments from John Davis and contain forms of the words "customer" and "support" in the description as a phrase.
1.9 Query Completion
The search box supports query completion. This feature suggests possible attributes and values that are based on your current input. When you start to enter in an empty search query YouTrack suggests attributes and values for keywords that match with the current input. You can select that keyword from the list using your mouse or keyboard. There are two modes of operation:

If you select an attribute the query completion suggests possible values for the selected attribute.
If you select a single value that value is added to the query.
1.10 Visual Indicators
When you enter a query in the search box YouTrack applies visual indicators to the query. This formatting tells you that your query has been parsed as a structured search.
Be careful to some warnings:

Issue attributes are shown as normal text without formatting.
Attribute values are shown in blue text.
Words or phrases that are parsed as a text search are shown in gold.
Invalid parameters are underlined in red.
2. Data Extrapolation
YouTrack REST Api
notes
header Authorization: 'Bearer {your_token}' is required for every request

use Accept HTTP header to indicate the expected response data: application/json

enpoint are exposed at: {your_youtrack_url}/api

for POST and PUT use Content-Type application/json

YouTrack supports OAuth2.0 but prefers the usage of a permanent token

eats and returns json data

you can add custom endpoints

Fields and Query Syntax
in YouTrack REST Api, when you sent a request to a resource, by default the servers sends back only the database ID and the $type of the resource entity. To receive attributes in response from the server you must explicitly specify the fields parameter.

Fields
fields allows you to choose the attributes you get the response.
Here's an example where we only want id, summary and project name of the issue:

https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)
the server will seturn something like:

[
    {
        'project':{
            'name':'Sample Project,
            '$type':'Project'
        },
            'summary':'1st issue of Sample Project',
            'id':'2-150',
            '$type':'Issue'
    },{
        'project':{
            'name':'Demo Project,
            '$type':'Project'
        },
            'summary':'1st issue of Demo Project',
            'id':'2- 120',
            '$type':'Issue'
    },
]
Custom Fields
CustomField is an entity that defines a common custom field wich contains basic attributs like name and fieldType.

If you want to see the custom fields of the issues you have to include customFields in the fields, you also have to specify which of the customField's attribute you want in your response

{your_url}/api/issues/?fields=id,summary,customFields(id,projectCustomField(field(name)),value(name))
Query
You can only use queries in GET requests that work with collections of issues or tags

query allows you to filter by the value of the attribute.
Here's an example where we want to see the issues of the project Sample Project.

https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)&query=project:+%7BSample+Project%7D

the response body will contain data only for the matching issues

[
    {
        'project':{
            'name':'Sample Project,
            '$type':'Project'
        },
            'summary':'1st issue of Sample Project',
            'id':'2-150',
            '$type':'Issue'
    }    
]
Here is a bit more complex query. we are looking for issues that are assigned to the user 'john.doe', not yet resolver, and contain the word 'summary'.

https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)&query=for:+john.doe+%23Unresolved+summary
Data obtained via URL
To retrieve data via URL:
https://{HOST:PORT}{url}

Example: retrieve attachments
To get the attachments of a specific issue we need the attachment's url.
To get the urls we'll take the list of attachments of the issue

/api/issues/{issue_id}/attachments
the response will look like this:

[
    {
        'charset':null,
        'extension':'png',
        'url':'/youtrack/api/files/7-2?sign=MTYxMzE2OTAwMDAwMHwxLTF8Ny0yfHdrRzJCYzA2cnZEclFLUzBLWEp6NXZGNE5kczA5eGVIVUUw%0D%0AbF9BVHZ3V3MNCg%0D%0A&updated=1569436739288",
        'mimeType':'image/png',
        'name':'attachment-name.png',
        'size':219293,
        '$type':'IssueAttachment'
    },
    .
    .
    .
    .
    .
]
now we can use the url in the attachment to receive the attachment.

https://{HOST:PORT}{url_in_attachments}
Note:
we don't need to include the token in the header of the request to retrieve the attachments because the client is identified using the sign= parameter in the url received from /api/issues/{issue_id}/attachments.

If we want to take all the attachments from the issues of the Project MioProject:

{base_url}/api/issues?fields=id,attachments(url)&query=project:MioProject
This will return a list of issues, each issue will have id and a list of attachments and each attachment will have his url

Entity Attributes
User
see official User documentation

id String

login String

fullName String

email String can be null

ringId String You can use this ID for operations in Hub and for matching users between YouTrack and Hub. can be null

guest Boolean

online Boolean

banned Boolean

tags Array of Tags

savedQueries Array of savedQueries

avatarUrl String

profiles UserProfiles

UserProfiles
see official UserProfiles documentation

id String

general GeneralUserProfile

notifications NotificationsUserProfile

timetracking TimeTrackingUserProfile

GeneralUserProfile
see official GeneralUserProfile documentation

id String

dateFieldFormat DateFormatDescriptor

timezone TimeZoneDescriptor

locale LocaleDescriptor

DateFormatDescriptor
see official DateFormatDescriptor decumentation

id String

presentation String (Name of the date format)

pattern String (pattern applied when showing date with time)

datePattern String

TimeZoneDescriptor
see official TimeZoneDescription documentation

id String

presentation String

offset Int

LocaleDescriptor
see official LocaleDescriptor documentation

id String

locale String (locale code, can be null)

language String (language abbreviation, can be null)

community Boolean (Indicated whether this locale is supported by community. if false the locale is official translation)

name String can be null

TimeTrackingUserProfile
see official TimeTrackingUserProfile documentation

id String

perdiodFormat PerdiodFieldFormat

PeriodFieldFormat
see official PeriodFieldFormat documentation

id String (available formats are FULL(week,day,howe,minutes) - DAYS_HOURS_MINUTES - HOURS_MINUTES - MINUTES)
NotificationsUserProfile
see official NotificationsUserProfile documentation

id String

notifyOnOwnChanges Boolean

emailNotificationsEnabled Boolean

mentionNotificationsEnabled Boolean

duplicateClusterNotificationsEnabled Boolean

mailboxIntegrationNotificationsEnabled Boolean

usePlainTextEmails Boolean

autoWatchOnComment Boolean

autoWatchOnCreate Boolean

autoWatchOnVote Boolean

autoWatchOnUpdate Boolean

UserGroup
see official UserGroup documentation

id String

name String

ringId String Use this ID for operations in Hub, and for matching groups between YouTrack and Hub. can be null

usersCount Long

icon String can be null

allUsersGroup Boolean

teamForProject Project can be null

Project
see official Project documentation

id String

archived Boolean

createdBy User

customFields ProjectCustomField

description String

fromEmail String

iconUrl String

issues Array of Issues

leader User

name String

replyToEmail String

shortName String

startingNumber Long

team projectTeam

template Boolean

ProjectTeam
*see official ProjectTeam documentation

project Project

id String

name String

ringId String

usersCount Long

icon String

allUsersGroup Boolean

teamForProject Project

Issue
see official Issue documentation

id String

attachments Array of IssueAttachments

comments Array of IssueComments

commentsCount Int

created Long

customFields Array of IssueCustomFields

description String can be null

draftOwner User (creator of the draft if the issue is a draft. null if the issue is reported. can be null)

externalIssue ExternalIssue can be null

idReadable String

isDraft Boolean

links Array of IssueLinks

numberInProject Long

parent IssueLinks can be null

pinnedComments Array of IssueComments

project Project can be null

reporter User can be null

resolved Long can be null

subtasks IssueLink

summary String

tags Array of Tags can be null

updated Long

updater User can be null

visibility Visibility can be null

voters IssueVoters

votes Int

watchers IssueWatchers

wikifiedDescription String

IssueAttachments
see official IssueAttachments documentation

id String

name String

author User can be null

created Long

updated Long

size Long

extension String can be null

charset String can be null

mimeType String can be null

metaData String can be null

draft Boolean

removed Boolean

base64Content String (can be null. Data URI that represents the attachment with the syntax: data:[][;base64],.
For example: "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==".)

url String can be null

visibility Visibility can be null

issue Issue can be null

comment IssueComment

thumbnailURL String can be null

IssueComments
see official IssueComment documentation

id String

attachments Array of IssueAttachments

author User can be null

created Long

deleted Boolean

issue Issue can be null

pinned Boolean

reactions Array of Reactions

text String can be null

textPreview String

updated Long can be null

visibility Visibility can be null

Reactions
see official Reactions documentation

id String

author User can be null

reaction String can be null

IssueCustomFields
see official IssueCustomFields documentation

id String

name String

projectCustomField ProjectCustomField

value Doesn't have a fixed Type it can also be a single value or an Array

ProjectCustomField
see official ProjectCustomField documentation

id String

field CustomField `can be null'

project Project can be null

canBeEmpty Boolean

emptyFieldText String can be null

ordinal Int

isPublic Boolean

hasRunningJob' Boolean (if true` there is a backgound job running for this field. In this case some field operations may be temporary inaccessible)

condition [CustomFieldCondition] (condition for showing the field can be null)

CustomField
see official CustomField documentation

id String

name String

localizedName string can be null

fieldType FieldType

isAutoAttached Boolean (if true the field will be automatically attached to new projects)

isDisplayedIsIssueList Boolean

ordinal Int (number of the field)

aliases String can be null

fieldDefaults [CustomFieldDefaults]

hasRunningJob Boolean

isUpdateable Boolean

instances Arrat of ProjectCustomFields

CustomFieldDefault
see official CustomFieldDefault documentation

id String

canBeEmpty Boolean

emptyFieldText String can be null

isPublic Boolean

parent CustomField

CustomFieldCondition
see official CustomFieldCondition documentation

id String

parend ProjectCustomField (the custom field that is hidden behind the condition. can be null)

FieldType
*see official FieldType documentation

id String (to see all available field types see Type Mapping for Custom Fields)
ExternalIssue
see official ExternalIssue documentation

id String

name String

url String can be null

key String can be null

IssueLink
see official IssueLink documentation

id String

direction Enumeration of: OUTWARD,INWARD and BOTH

linkType IssueLinkType can be null

issues Array of Issues

trimmedIssues Array of Issues

IssueLinkType
see official IssueLinkType documentation

id String

name String

localizedName String can be null

sourceToTarget String

localizedSourceToTarget String can be null

targetToSource String can be null

localizedTargetToSource String

directed Boolean

aggregation Boolean

readOnly Boolean

IssueVoters
*see official IssueVoters documentation

id String

hasVote Boolean

original Array of Users

duplicate Array of DuplicateVotes

DuplicateVote
*see official DuplicateVote documentation

id String

issue Issue can be null

user User can be null

IssueWatchers
see official IssueWatchers documentation

id String

hasStar Boolean

issueWatchers Array of IssueWatchers

duplicateWatchers Array of IssueWatchers

Visibility
see official Visibility documentation
Visibility has no attributes, but is extended by UnlimitedVisibility and LimitedVisibility

UnlimitedVisibility
UnlimitedVisibility has no attributes.

LimitedVisibility
id String
permittedGroups Array of UserGroups
permittedUsers Array of Users
Tags
see official Tag documentation

id String

issues Array of Issues

color FieldStyle

untagOnresolve Boolean

visibleFor UserGroup can be null

updateableBy UserGroup can be null

readSharingSettings WatchFolderSharingSettings

tagSharingSettings TagSharingSettings

updateSharingSettings WatchFolderSharingSettings

owner User

name String

FieldStyle
see official FieldStyle documentation

id String

background String can be null

foreground String can be null

WatchFolderSharingSettings
see official WatchFolderSharingSettings documentation

id String

permittedGroups Array of UserGroups

permittedUsers Array of Users

TagSharingSettings
see official TagSharingSettings documentation

id String

permittedGroups Array of UserGroups

permittedUsers Array of userGroups

SavedQueries
*see official SavedQueries documentation

id String

query String can be null

issues Array of Issues

visibleFor UserGroup can be null

updateableBy UserGroup can be null

readSharingSettings WatchFolderSharingSettings

updateSharingSettings WatchFolderSharingSettings

owner User can be null

name String
