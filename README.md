# EdVisingU-HiredEd
HireEd Labs is an automation hub powering EdVisingU’s multi-store AI education ecosystem. Built with n8n, APIs, and MailerLite, it connects Whop, Etsy, Gumroad, Skillplate, and live streams into a unified revenue engine—automating product creation, CRM onboarding, lead capture, KPI tracking, and student-built AI ventures.

# Sprint 1

Sprint 1 mainly consisted of researching and coordinating with the other EdVisingU team and sponsor to determine the technical and platform infrastructure. We decided on Lovable as the foundation for development.

Research:

David Elkins
Twitch API 
The Twitch API is an interface designed for interacting with Twitch, enabling us to retrieve real-time data about live streams, channel information, user profiles, and chat metadata, as well as perform moderation actions and manage channel settings through OAuth 2.0 authenticated endpoints. It supports both read operations and write operations, and also offering EventSub for webhook-based real-time event notifications when specific channel activities occur.
Authentication
OAuth 2.0
Token access via online portal 
Endopoints
api.twitch.tv/helix
/token - access to current token and can get client ID, client secrets, grant type, and scope
/revoke - removes any token access that is passed to it
/validate - checks if a token is valid or not
/users
/follows - gets who the user follows between another user
/extensions - get a list of all extensions the user has
/streams - gets information about the current stream, view count, title, game, and start time
/markers - gets a list of all steam markers from the specified stream
/channels - gets the information for a channel
/commercial - Starts commercial on channel specified
/chat - base for all chat messages
/emotes - get emotes for one or more channels
/set get emotes for channel
/badges - get list of custom change badges for channel
/global - get list of global badges
/settings get chat setting for channel
/moderation - base for all moderation
/banned - get list of all banned users
/bans - post - bans user from channel
/bans - delete - unbans user
/moderators - gets list of all moderators for channel
/eventsub - base for when a subscription happens
/subscriptions - get - get list of all sub events
/subscriptions - delete - removes a subevent from events
/games - get information about game
/top - get list of top games listed by number of viewers
/videos - get information about video
/clips - creates a clip 

Etsy
The Etsy API v3 is a RESTful OpenAPI-compliant interface that facilitates programmatic access to the Etsy marketplace, allowing us to manage the store operations including shop configuration, listing creation and inventory management, order processing through receipts, and user profile data using OAuth 2.0. It supports both read operations and authenticated write operations, while providing webhook capabilities to receive real-time notifications for order events and listing updates, that we will use in n8n.

Authentication
OAuth 2.0 with PKCE for authorization flows
Token access via online portal 
Endopoints
api.etsy.com/v3/applicaiton
/shops - get list of shops by id
/{shop id} - get single shop detail
/sections - get shop categories
/about - gets the about page of the store
/listing - gets the list of listings for the shop
/{shop id}/receipts - get list of receipts for shop
/listings - base for individual listings
{listing id} - get information about a single listing
/batch - gets listing by id
/images - upload or takedown images for listing
/inventory - set and get the number of items available for listing
/attributes - set and get description and other data about the listing
/files - set and get files for digital listings (We will use this one alot!!)
/receipts - base for order receipts
/{receipt id} - get information about a single receipt
/transactions - retrieves transactions within receipts
/users -  base for users
/{user id} - gets profile information
/me - gets information about the user the whos access token this is
/address - gets or set my user address information
/taxonomy - base for tax related things
/nodes - gets information about all the taxonomy nodes under id
/attributes/{taxonomy id} - gets all attributes for category
/reviews - get review for id that is passed in
/shipping-profiles - gets shipping rates
/{profile id} - gets details about specific shipping profile
/distination - gets shipping information for delivery to specific address
/webhooks - gets information about all the webhooks inuse for subsciptions
 CrediVersity Whop API
The Whop Store API for CrediVersity implements a RESTful interface with webhook events designed for n8n automation. The specification defines endpoints for subscription management, tier-based access control, and events streaming while streaming on Twitch. The API will falow the  OpenAPI specification with JSON payload. Webhook endpoints operate on a subscription model where CrediVersity registers callback URLs to receive push notifications for business events, eliminating the need for polling architectures in the n8n Automation Hub. We are drafting up what endpints we need and how exactly we will integrat it into the entire system

n8n integration
Twitch 
n8n has a native Twitch integration, which provides direct access to the API without requiring manual API configuration. This native integration supports authentication, allowing n8n to securely connect to Twitch using credentials obtained from the Twitch Developer. The n8n node enables workflows to perform operations such as retrieving user information, checking stream status, fetching channel details, and accessing chat. For real-time automation, like flash sales, n8n utilizes Twitch's EventSub system, which pushes notifications to n8n's webhook endpoint when specific events occur, such as a stream going live or a subscription event.
Etsy
The Etsy integration in n8n requires making a custom HTTP Request node since there is no native Etsy node available in n8n. This integration utilizes the Etsy API v3, which follows OpenAPI 3.0 standards and implements OAuth 2.0. For public readonly operations such as searching listings or viewing shop details, the integration can utilize a simple API Key, while private operations like managing orders, listings, and inventory require full OAuth 2.0 authentication with appropriate scopes. The workflow architecture will employ a polling approach because there is no webhook for most of the API. Polling will use the Schedule Trigger node to query the Etsy API for new orders or listing updates. For some endpoints Etsy has webhooks and those can be configered to send a even to other nodes for specific events, allowing the automation hub to react to new purchases or listing modifications.


Whop OpenAPI

The CrediVersity Whop team will be implementing the OpenAPI standard to allow for communication between their Whop store and the n8n Automation Hub, removing the need to build a custom integration components from the ground up. By using the OpenAPI specifications, both teams will have a basis of a well-documented specification that defines available endpoints, request parameters, response schemas, and authentication requirements. This will allow us to use n8n to automatically discover and utilize the Whop store without manual configuration, helping to  reducing development time and minimizing the risk of errors. The Whop store will expose endpoints for managing subscription tiers, processing purchases, tracking member entitlements, and handling webhook event subscriptions. These will be utalized by the n8n Automation Hub to trigger other nodes such as MailerLite, Etsy, Google Gemini, and Twitch notifications. The OpenAPI specification will ensure secure data transmission between systems while allowing full Whop integration. Webhook events will real time communication between nodes. 

Ethan Florendo
Skillplate Integration Options
Skillplate will be the storefront where the different offers will be displayed. Doesn’t appear to be a public API or webhook documentation like YouTube or Etsy; backend automation will be limited. The best way to use Skillplate in our Automation Hub is likely as a central catalog that will link to Whop, Etsy, and Gumroad. n8n handles the actual purchase and CRM automation through those platforms.
●      Little options for automation with API/ Webhook
●      Catalog that links to Whop, Etsy, and Gumroad.
Map Data Schemes Across Platforms
Prioritization changed from integration of owner’s store and automations to Student profiles for the portfolio website. This data reflects the new student portfolio data schemes.

Table/Collection
Field Name
Data Type
Notes
students
id
UUID (PK)
Primary key
students
email
string (unique)
Student email for auth
students
name
string
Full name
students
cohort_id
UUID (FK)
Links to cohorts table
students
status
enum: pending|active|alumni
Enrollment status
students
created_at
timestamp
Account creation date








talent_profiles
id
UUID (PK)
Primary key
talent_profiles
student_id
UUID (FK)
Links to students table
talent_profiles
display_name
string
Public display name
talent_profiles
program
string
e.g., "Computer Science, ASU Spring 2026"
talent_profiles
bio
text
Short bio in "disruptive professor" tone
talent_profiles
skills
array<string>
e.g., ["AI", "Full-stack", "Automation"]
talent_profiles
tech_stack
array<string>
e.g., ["React", "n8n", "Supabase", "Python"]
talent_profiles
github_url
string (URL)
Optional GitHub profile link
talent_profiles
linkedin_url
string (URL)
Optional LinkedIn profile link








projects
id
UUID (PK)
Primary key
projects
profile_id
UUID (FK)
Links to talent_profiles table
projects
title
string
Project name
projects
description
text
Brief project description
projects
project_type
enum: etsy_store|gumroad_store|n8n_workflow|automation
Type of deliverable
projects
url
string (URL, optional)
Link to live store/repo/demo








applications
id
UUID (PK)
Primary key
applications
name
string
Applicant name
applications
email
string
Applicant email
applications
program_interest
string
What they want to learn/build
applications
message
text
Why they want to join
applications
status
enum: new|reviewed|accepted|rejected
Application review status
applications
created_at
timestamp
Submission timestamp








inquiries
id
UUID (PK)
Primary key
inquiries
organization
string
Company/institution name
inquiries
contact_name
string
Contact person name
inquiries
email
string
Contact email
inquiries
inquiry_type
enum: hire|sponsor|partner
What they are interested in
inquiries
message
text
Inquiry details
inquiries
created_at
timestamp
Submission timestamp








cohorts
id
UUID (PK)
Primary key
cohorts
name
string
Cohort name (e.g., "Spring 2026")
cohorts
phase
enum: 1|2
Phase 1 or Phase 2
cohorts
start_date
date
Cohort start date
cohorts
end_date
date
Cohort end date
cohorts
status
enum: upcoming|active|completed
Current cohort status



Youtube API
https://developers.google.com/youtube/v3/docs
The YouTube API is an interface for interacting with YouTube, allowing us to retrieve and manage data about videos, channels, playlists, comments, subscriptions, and live streams. It supports both read (search, list, analytics-like metadata) and write (upload videos, create playlists, manage comments, create live broadcasts/streams) operations, which are done through authenticated requests.
Authentication
OAuth for user private data and write actions, managing live streams, reading a user’s subscriptions, etc.
API key for public-data read requests
Token access via online portal: Gcreate OAuth client + configure OAuth consent screen/scopes
Scopes
https://www.googleapis.com/auth/youtube.readonly (read)
https://www.googleapis.com/auth/youtube (manage account)
https://www.googleapis.com/auth/youtube.upload (upload)
Endopoints
Base: https://www.googleapis.com/youtube/v3
/search
Search across YouTube content using keywords
/live - filter for live broadcasts
/videos
Get video details title, description, stats, duration
/statistics - views, likes, comments
/contentDetails - duration, live status
/liveStreamingDetails - concurrent viewers, start time
/channels
Get channel info
/statistics - subscriber count, view count, video count
/contentDetails - includes uploads playlist ID
/playlistItems
List items inside a playlist
/playlists
Create/list/manage playlists
/items - gets the list of videos in a playlist
/subscriptions
Get or manage subscriptions for the authenticated user
/list - gets a list of channels a user is subscribed to
/insert - subscribes the authenticated user to a channel
/comments
Create/update/delete a specific comment
/commentThreads
Read comment threads on videos and channels
/list - gets a list of comments for a video
/insert - posts a new top-level comment
/captions
Manage caption tracks
/thumbnails
Upload and set custom thumbnails for a video
/liveBroadcasts
Create/manage live broadcasts
/list - gets a list of scheduled or active broadcasts
/transition - changes the status of a live broadcast
/status - broadcast status
/contentDetails - broadcast settings
/liveStreams
Create/manage ingestion streams
/liveChatMessages
Read and insert live chat messages
/{live_chat_id} - retrieve chat messages
/liveChatBans
Ban/unban users from live chat
/{live_chat_id} - manage banned users
Mailerlite API
The MailerLite API automates email marketing by synchronizing subscribers, managing groups, and triggering automated campaigns. It supports read operations, pull subscribers, groups, and write operations, add/update subscribers, and create campaigns. It also offers Webhooks to receive real-time notifications.
Authentication
API Key
MailerLite uses an API key for authenticating requests. This is passed as a request header 
Endopoints
Base: https://connect.mailerlite.com/api
/subscribers - access and manage subscriber records
/{subscriber_id} - get subscriber by ID
/forget - permanently remove subscriber data
/groups - sub groups
/{group_id} - access specific group
/subscribers - list subscribers in group
/subscribers/{subscriber_id} - remove subscriber from group
/segments - segmentation
/{segment_id} - get segment details
/subscribers list subscribers in segment
/campaigns – email campaigns
/{campaign_id} - get campaign details
/reports - get campaign performance data
/reports/subscriber-activity - get engagement activity
/reports/link-activity - get click tracking data
/automations - automation workflows
/webhooks - event trigger system






Nathan Lepacik

Gumroad API

Authentication
 OAuth 2.0 (Authorization Code flow)
 Access token via OAuth redirect (no separate token endpoint like Twitch)
 Personal access token via Gumroad dashboard (for simple API key usage)
Endpoints
	api.gumroad.com
/oauth/authorize - redirects user to authorize your application
	/oauth/token - exchanges authorization code for access token
/sales - get list of sales (can filter by product, email, date, etc.)
	/sales/{id} - get information about a specific sale
	/sales - post - verify a sale using product permalink and license key
/products - get list of products for authenticated user
	/products - post - create a new product
	/products/{id} - get information about a specific product
	/products/{id} - put - update a product
	/products/{id} - delete - delete a product	
/licenses/verify - verify a license key for a product
/subscribers - get list of subscribers
	/subscribers - post - add a subscriber to an audience
/offers - create or manage discount offers for products
/refunds - refund a specific sale
/user - get authenticated user information

Calendly API
Authentication
 OAuth 2.0 & Personal Access Tokens
 • OAuth for external apps (Authorization Code flow with access/refresh tokens)
 • Personal access token via dashboard for internal use
 All calls use Bearer tokens in the Authorization header.
Endpoints
	Base URL: https://api.calendly.com
/oauth/authorize - get authorization code for OAuth apps
	/oauth/token - exchange code for access + refresh tokens
	/oauth/revoke - revoke a token
/users
	/users/me - get info about authenticated user
	/users/{uuid} - get a specific user
/organization_memberships
	/list - list memberships for a user/org
	/{uuid} - get specific membership details (used to manage organization users)
/organizations
	/{uuid} - get organization details (contains link to event types, members, etc.)
/event_types
	/list - list event types (for user/org)
	/{uuid} - show details of an event type
	/create - create a new event type
	/update - update event type
	/delete - delete event type
/event_type_available_times
	/list - list available times for an event type (filter by date range)
/scheduled_events
	/list - list scheduled events
	/{uuid} - get a specific scheduled event
/invitees
	/list - list invitees across events
	/{uuid} - get info for a specific invitee
/cancel_scheduled_event - POST - cancel a scheduled event (delete/cancel actions for invitee/event)
/availability_schedules
	/list - list user availability schedules
	/{uuid} - get a specific availability schedule
	/update - update an availability schedule
/busy_times
	/list - get list of busy times (for user/org in date range)
/event_type_pools
	/list - list event type pools
	/{uuid} - get event type pool details
/webhook_subscriptions
	/list - list webhook subscriptions
	/create - create a webhook subscription
	/{uuid} - get details for a subscription
	/{uuid} - delete a subscription
/data_compliance/delete_invitee_data - POST - delete invitee PII (enterprise)
	/data_compliance/delete_scheduled_event_data - POST - delete scheduled event data (enterprise)
/activity_log_entries
	/list - list activity entries (enterprise audit logs)

Google AI Studio (Gemini)

Authentication
API Keys
• Create API key in Google AI Studio
• Send via x-goog-api-key header
• Can also pass ?key=API_KEY query parameter (server-side recommended)
No OAuth required
All requests authenticated with API key

/models
GET - list available models
GET /{model} - get model details

/models/{model}:generateContent
POST - generate content (text, chat, multimodal)

/models/{model}:streamGenerateContent
POST - stream generated content

/models/{model}:countTokens
POST - count tokens for input

/models/{model}:embedContent
POST - generate embeddings

/models/embedding-001:batchEmbedContents
POST - batch embeddings

/files
GET - list uploaded files
POST - upload file
GET /{file} - get file metadata
DELETE /{file} - delete file

/cachedContents
GET - list cached contents
POST - create cached content
GET /{cachedContent} - get cached content metadata
DELETE /{cachedContent} - delete cached content

/tunedModels
GET - list tuned models
POST - create tuned model
GET /{tunedModel} - get tuned model details
DELETE /{tunedModel} - delete tuned model

/tuningJobs
GET - list tuning jobs
GET /{tuningJob} - get tuning job details
POST /{tuningJob}:cancel - cancel tuning job
