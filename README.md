# Prompt-


FLIKZO — MASTER PRODUCT ENGINEERING & DEVELOPMENT PROMPT

1. ROLE AND MISSION

Act as a senior Android engineer, backend engineer, database architect, product designer, DevOps engineer, QA engineer, security engineer, and technical product owner.

I am developing Flikzo, a production-oriented short-video sharing platform inspired by the general user experience of TikTok and Instagram Reels.

The initial Android APK has already been generated and successfully installed on a physical Android phone. The current implementation is only a basic foundation. The final UI/UX and most application features have not been implemented.

Your task is to evolve the existing project into a maintainable, secure, testable, scalable short-video platform through small, independently verifiable development phases.

Do not assume the existing code is correct or complete. Inspect the actual repository and installed application before making changes.

Do not replace the entire project with a new template merely because that is easier.

2. NON-NEGOTIABLE DEVELOPMENT RULES

1. Start by auditing the existing repository, current Git branch, commit history, build configuration, Android package ID, Gradle setup, dependencies, existing backend, database migrations, and available tests.
2. Identify which components already work, which are placeholders, and which are missing.
3. Preserve working functionality unless a documented reason justifies changing it.
4. Do not claim that an API, database, feature, payment, or admin control works unless it has been implemented and tested.
5. Do not use fake backend responses, hardcoded user statistics, fake payment success, or simulated production data as substitutes for real implementations.
6. Use clean architecture and modular boundaries appropriate to the existing project. Avoid unnecessary abstraction and premature microservices.
7. Every phase must have a clear scope, implementation tasks, automated tests, acceptance criteria, and a rollback point.
8. After every meaningful feature group and every completed phase, build a new APK, install it on a physical Android phone, launch it, and test the affected functionality.
9. A successful APK build is not proof that the application works correctly.
10. Do not mark a phase complete until its mandatory acceptance tests pass or a blocker is explicitly documented.
11. Never claim that an APK was installed or tested on a physical device unless that actually happened.
12. If no physical Android device or installation mechanism is available, provide the exact commands and mark the physical-device test as BLOCKED. Do not fabricate results.
13. Keep the repository buildable after each phase.
14. Work incrementally. Do not attempt to implement the entire platform in one massive code change.
15. If a critical architectural decision is unclear, inspect the codebase and document the decision before proceeding.

3. PRODUCT VISION

Flikzo should provide a polished, fast, creator-friendly short-video experience.

Primary product objectives:

- Make it easy to discover and watch interesting videos.
- Make video creation and publishing simple.
- Help creators understand their audience and performance.
- Support meaningful interactions and community building.
- Provide transparent, controlled creator-reward campaigns.
- Give authorized administrators comprehensive control over the platform.
- Protect users against harassment, spam, fraud, and unauthorized access.
- Keep infrastructure costs appropriate for an early-stage product.
- Allow the platform to grow without requiring a complete rewrite.

Target the MVP at hundreds to several thousand users, but design the data model and API boundaries so future growth remains practical.

4. TECHNOLOGY STACK

Inspect the current repository and preserve compatible choices.

Android

- Kotlin
- Jetpack Compose
- Material 3
- Navigation Compose
- ViewModel
- Coroutines and Flow/StateFlow
- Hilt or the project's existing dependency-injection solution
- Retrofit/OkHttp or a consistent existing HTTP client
- Kotlin Serialization
- Media3/ExoPlayer for video playback
- Room for appropriate local persistence
- DataStore for app preferences
- Paging for scalable feed loading
- WorkManager for suitable persistent background work

Use stable, mutually compatible dependency versions. Do not blindly upgrade every dependency.

Backend

- Kotlin
- Ktor
- PostgreSQL
- Flyway database migrations
- HikariCP connection pooling
- Parameterized database queries
- Structured logging
- Versioned REST APIs
- Secure authentication and server-side authorization

Choose Exposed or jOOQ after auditing the existing backend. Do not introduce multiple competing database-access frameworks without a clear reason.

Infrastructure

- Docker and Docker Compose for reproducible development
- Private S3-compatible object storage for video files and images
- FFmpeg-based background video processing
- CDN integration when appropriate
- GitHub Actions for automated build and test workflows
- Redis only when measured performance or a specific requirement justifies it
- PostgreSQL-backed jobs or an outbox pattern for initial asynchronous work

Do not introduce Kubernetes, Kafka, Cassandra, or distributed microservices into the MVP without demonstrated need.

5. PHASE 0 — REPOSITORY AUDIT AND BASELINE APK

Before implementation:

1. Inspect all relevant project files.
2. Identify the actual Android application module and application ID.
3. Identify the current package structure, navigation, theme, and existing screens.
4. Inspect Gradle configuration and determine the current supported Android SDK range.
5. Check backend and database status, if present.
6. Run the existing test suite and baseline build.
7. Record known failures and existing limitations.
8. Produce a short architecture assessment.
9. Confirm whether the current APK can still be reproduced from the source.
10. Save a baseline Git commit or a recoverable checkpoint.

Deliverables:

- Repository audit
- Existing feature inventory
- Build and test results
- Architecture gaps
- Ordered implementation plan
- Baseline APK artifact, if buildable

Do not redesign the entire application in this phase.

6. PHASE 1 — DESIGN SYSTEM AND UI/UX

Create a consistent Material 3 design system before implementing every feature.

Define:

- Color tokens
- Typography
- Spacing
- Shapes and corner radii
- Icon conventions
- Light and dark themes
- Button, input, dialog, bottom-sheet, and card components
- Loading, empty, error, offline, and retry states
- Accessibility and contrast requirements

Create a modern visual identity for Flikzo. Keep app branding remotely configurable where appropriate, but do not assume changing the app title remotely changes the installed Android package name or launcher icon.

Main navigation

Implement:

- Home
- Discover/Search
- Create/Upload
- Inbox/Notifications
- Profile

Home feed

- Full-screen vertical video experience
- Smooth swipe navigation
- One active playback item at a time
- Appropriate adjacent-video preloading
- Play/pause and mute controls
- Like, comment, share, and follow actions
- Creator identity and caption
- Sound information when available
- Clear loading, error, and retry states
- Correct handling of empty feeds
- Proper lifecycle and audio-focus behavior

Other screens

- Search and discovery
- User profile
- Video details
- Comment sheet
- Notifications
- Video creation and upload
- Settings and privacy
- Login and registration when required
- Creator analytics
- Rewards dashboard when the reward feature is implemented

Use reusable composables and unidirectional UI state flow.

Do not use placeholder navigation that leads to dead screens.

Mandatory gate

- Run Android unit tests and Compose/UI tests.
- Build the APK.
- Install it on a physical phone.
- Open every implemented screen.
- Test scrolling, navigation, back behavior, keyboard handling, loading, and errors.
- Inspect different screen sizes and dark/light themes.
- Fix crashes, clipping, incorrect system-bar handling, and major performance problems.
- Record the tested commit and APK version.

7. PHASE 2 — AUTHENTICATION AND ACCOUNT SECURITY

Implement a complete authentication flow.

Requirements:

- Registration and login
- Secure password hashing using Argon2id or another justified secure password-hashing implementation
- Short-lived access tokens
- Refresh-token rotation and revocation
- Logout and session management
- Email verification and account recovery where supported
- Rate limiting on sensitive endpoints
- Server-side input validation
- Authorization on every protected resource
- Secure handling of authentication failures
- Account suspension and deactivation states

Never store plain-text passwords, raw refresh tokens, or secrets in source code.

Do not trust a user ID supplied by the Android client to determine the authenticated user. Derive identity from validated authentication credentials.

Database foundation

Use PostgreSQL with Flyway migrations.

Implement appropriate tables for:

- Users
- User profiles
- Refresh-token sessions
- Account status
- Administrative roles and permissions

Use UUID identifiers where appropriate, foreign keys, unique constraints, CHECK constraints, and indexes based on actual query patterns.

Mandatory gate

- Unit-test authentication and authorization logic.
- Integration-test login, logout, token refresh, expired sessions, duplicate registrations, and invalid credentials.
- Test protected endpoints using unauthorized and incorrectly authorized requests.
- Build and install the APK on a physical phone.
- Verify login persistence, logout, app restart, token expiry, and recovery behavior.
- Do not proceed while critical authentication vulnerabilities remain unresolved.

8. PHASE 3 — VIDEO UPLOAD, STORAGE, AND PLAYBACK

Implement real video handling rather than local-only demo playback.

Requirements:

- Upload-session creation
- Upload progress and cancellation
- File-size and duration limits
- Supported format validation
- Secure upload URLs
- Upload completion verification
- Persistent upload state where appropriate
- Thumbnail generation
- Video metadata extraction
- Background transcoding
- Processing retries
- Processing failure states
- Safe cleanup of abandoned uploads
- Private original-file storage
- Playback delivery using appropriate streaming formats and CDN integration when available

Use object storage for actual video files. Store metadata and storage keys in PostgreSQL rather than placing large video binaries in ordinary relational rows.

Implement an explicit processing lifecycle, such as:
UPLOADING → PROCESSING → READY

Also support FAILED and REMOVED states.

Never expose an unprocessed upload publicly.

The backend must verify object ownership, expected size, file format, and upload completion. Do not trust the client-reported MIME type or dimensions.

Use FFmpeg inside a restricted worker environment. Set appropriate CPU, memory, disk, execution-time, and file-size limits.

Do not proxy large video uploads through Ktor unless there is a documented technical requirement.

Mandatory gate

- Test small and large allowed uploads.
- Test cancellation, network loss, retry, expired upload URLs, and failed processing.
- Test corrupt files and unsupported formats.
- Verify that unfinished videos do not appear in the public feed.
- Verify playback and seeking on a physical Android phone.
- Test portrait and landscape videos and supported playback renditions.
- Measure upload and initial-playback behavior under slower network conditions.
- Build, install, and regression-test the APK.

9. PHASE 4 — CORE SOCIAL FEATURES

Implement:

- Like and unlike
- Comments and replies
- Follow and unfollow
- Public and private profile settings
- Video visibility settings
- User blocking and reporting
- Comment moderation
- Notifications
- Video sharing
- Creator profile pages
- Creator video management
- Pagination and refresh behavior

Use server-side database constraints to prevent duplicate likes and follows.

Design endpoints to handle retries and repeated requests safely.

Enforce privacy and visibility rules on the backend, not only in the UI.

Prevent users from modifying or deleting other users' content unless explicitly authorized.

Mandatory gate

- Test duplicate and concurrent like/follow requests.
- Test comments, replies, deletion, and moderation.
- Test private profiles and restricted content.
- Test blocked-user interactions.
- Verify notifications are not duplicated unnecessarily.
- Build and install the APK and test all social interactions on a physical phone.

10. PHASE 5 — ADMIN PANEL AND REMOTE CONFIGURATION

Create a responsive web-based administration dashboard. It must work on phone, tablet, and desktop browsers.

The dashboard is a separate administrative client connected to protected backend APIs. It must not bypass backend security.

Where appropriate, expose a restricted administrative interface inside the Android app for authorized staff, but keep privileged operations protected by backend authorization.

Admin dashboard modules

A. Overview:

- Total registered users
- Daily and monthly active users
- New users
- Uploads and published videos
- Validated views
- Engagement statistics
- Creator-reward liability
- Storage usage
- API and worker health

B. User management:

- Search and filter users
- View permitted account information
- Suspend and restore accounts
- Review reports and appeals
- Manage authorized roles
- Process account deletion requests

C. Content moderation:

- Report queue
- Video review
- Takedown and restoration
- Content visibility controls
- Moderation reasons
- Appeals and review history

D. Remote configuration:

- Display name and tagline
- Remote logo and other supported branding assets
- Primary and secondary colors
- Onboarding announcements
- Feature flags
- Maintenance mode
- Upload limits
- Supported content restrictions
- Feed and notification configuration

E. Reward management:

- Campaign creation and scheduling
- Reward eligibility rules
- Budget and spending limits
- Reward approvals
- Payout requests
- Reconciliation and audit history

F. Platform operations:

- API health
- Worker failures
- Storage metrics
- Database health
- Backup status
- Error summaries
- Deployment and migration information

Security requirements

Implement role-based access control with appropriate least-privilege roles, such as:

- SUPER_ADMIN
- ADMIN
- MODERATOR
- FINANCE_OPERATOR
- SUPPORT

The exact permissions must be explicit and enforced server-side.

Require stronger authentication and additional confirmation for sensitive financial and account-security actions.

Record administrative changes in an audit log.

Do not permit a moderator to approve financial withdrawals merely because they can moderate videos.

Remote configuration must be validated, versioned, and published through the backend. Include draft, preview, publish, and rollback capabilities where appropriate.

A maintenance-mode switch must have an explicit client behavior and a controlled staff bypass. It must not disable emergency access or essential account-security controls.

Mandatory gate

- Test dashboard access using every role.
- Verify that unauthorized API requests are rejected.
- Test configuration draft, publish, fetch, rollback, and invalid configuration.
- Test on mobile, tablet, and desktop browser sizes.
- Verify audit records for sensitive actions.
- Build and install the Android APK and confirm that published remote configuration reaches the app.
- Verify that changing remote branding does not break the UI or falsely claim to change the package ID or native launcher icon.

11. PHASE 6 — VIEWS, ANALYTICS, AND CREATOR DASHBOARD

Implement real analytics.

Track appropriate playback events:

- Playback started
- Validated view
- Watch duration
- Completion
- Likes
- Comments
- Shares
- Follows

Use event IDs and idempotent processing to avoid double counting.

Do not treat every video impression, playback tick, or API request as a valid view.

Implement:

- Total validated views
- Unique viewers where reliably measurable
- Watch-time summaries
- Average watch duration
- Completion rate
- Engagement statistics
- Follower growth
- Time-based charts

Separate raw playback events from aggregated statistics.

For the MVP, PostgreSQL may hold raw events and aggregate tables. Avoid introducing a dedicated analytics warehouse before real usage requires one.

Mandatory gate

- Test duplicate event IDs.
- Test malformed and suspicious playback events.
- Verify that retries do not double-increment counters.
- Compare aggregate counts against controlled test data.
- Test creator ownership restrictions.
- Build and install the APK and verify analytics on a physical phone.

12. PHASE 7 — CREATOR REWARD CAMPAIGNS

Build the reward system as a controlled financial workflow, not a simple client-side balance counter.

Initial campaign example:
A creator's video may qualify for a ৳100 reward after reaching 1,000 validated views and 100 qualifying likes, subject to published campaign rules and fraud review.

These thresholds are configurable examples, not a guarantee that every video will automatically receive payment.

Required controls

- Campaign start and end dates
- Eligibility conditions
- Per-video and per-user limits
- Total campaign budget
- Pending and approved rewards
- Rejected rewards with recorded reasons
- Payable and paid balances
- Withdrawal requests
- Payment references
- Reconciliation
- Administrative approvals
- Immutable or auditable ledger entries

Maintain a financial ledger and separate reward states. Never create money by trusting values supplied by the Android app.

Use database transactions, unique constraints, idempotency keys, and audit logs to prevent duplicate rewards or payments.

Implement fraud controls for:

- Repeated artificial engagement
- Bot-like traffic
- Suspicious account clusters
- Repeated self-generated engagement
- Repeated payment identities where lawful and appropriate to assess
- Unusual engagement velocity
- Coordinated abuse

Use proportionate review and appeal procedures. Do not automatically accuse a creator of fraud based solely on one suspicious signal.

Before enabling real payouts, verify applicable Bangladesh payment-provider, merchant, tax, consumer-protection, and other legal requirements. Integrate only supported payout methods and never store payment PINs or OTPs.

Start with a small, explicitly funded pilot. Make eligibility terms and reward availability transparent.

Mandatory gate

- Test eligibility boundaries and budget exhaustion.
- Test duplicate requests and concurrent reward calculations.
- Test cancellation, rejection, and payout reconciliation.
- Verify that users cannot alter their balance.
- Verify that admin permissions are separated.
- Build and install the APK and test the creator dashboard and reward states.

Do not mark real-money payments complete until a real supported payment integration has been implemented and verified.

13. PHASE 8 — DISCOVERY AND GROWTH

Build discovery features that improve genuine engagement and retention.

Prioritize:

- Chronological feed as a reliable initial fallback
- Trending videos based on validated engagement
- Search for creators, captions, and hashtags
- Personalized recommendations using watch behavior
- Interest selection during onboarding
- Related creator discovery
- Follow-based feed
- Push notifications with user controls
- Shareable video links
- Creator onboarding
- Referral attribution and abuse prevention
- Campaign analytics

Avoid building a complex recommendation microservice for the MVP.

Begin with transparent ranking signals and a fallback feed. Measure meaningful outcomes such as successful playback, watch time, completion, return visits, and user feedback.

Do not optimize exclusively for time spent in the app. Provide controls for notifications, content preferences, and unwanted interactions.

Never purchase fake views, bots, or fabricated engagement to inflate growth statistics.

14. PHASE 9 — MODERATION, PRIVACY, AND TRUST

Implement:

- Report video and report user
- Block and mute controls
- Comment moderation
- Content removal
- Account suspension and appeal
- Copyright complaint handling
- Privacy settings
- Account export and deletion workflow where applicable
- Data-retention policies
- Rate limits
- Abuse monitoring
- Secure administrative audit trails

Minimize collected personal data.

Do not expose private email addresses, viewing histories, internal moderation notes, payment details, or authentication tokens through public endpoints.

Define content rules, enforcement procedures, and user-facing explanations before inviting large numbers of users.

15. PHASE 10 — PERFORMANCE, RELIABILITY, AND RELEASE

Implement:

- Automated unit and integration tests
- Android UI tests for critical journeys
- Backend API tests
- Database migration tests
- Build automation
- Crash and error monitoring
- Structured logs
- Readiness and liveness checks
- Database backup and restoration tests
- Network failure handling
- Pagination
- Resource limits
- Dependency and security checks
- Release signing
- Versioning
- Staged releases and rollback procedures

Run load tests against representative MVP workloads before making performance claims.

Do not assume that a passing unit-test suite proves production readiness.

Never expose production secrets in GitHub Actions logs or repository files.

Keep signing credentials and service credentials in appropriately protected secret stores.

16. MANDATORY APK BUILD AND PHYSICAL-DEVICE TESTING GATES

This requirement applies to every phase, including UI changes, API integration, authentication, uploads, feed behavior, analytics, rewards, remote configuration, and release preparation.

After each completed feature group:

1. Review the changed files.
2. Run relevant formatting, lint, and static-analysis checks.
3. Run unit and integration tests.
4. Build the debug APK using the repository's actual Gradle configuration.
5. Confirm the build exit status and locate the generated APK.
6. Install the APK on a physical Android phone.
7. Launch the app and execute the affected user journey.
8. Inspect Logcat and backend logs for crashes and errors.
9. Test loading, empty, success, failure, retry, and network-disconnection states as applicable.
10. Check regression in existing functionality.
11. Fix discovered bugs and repeat the build and test process.
12. Save test evidence and record the Git commit.
13. Preserve a known-good APK for comparison or rollback.

Use the actual project Gradle wrapper. Do not invent task names or APK paths.

Typical commands, to be adapted to the actual repository:

./gradlew test
./gradlew lint
./gradlew assembleDebug

For a connected physical device, after confirming the device is authorized:

adb devices
adb install -r <verified-apk-path>
adb shell monkey -p <actual-application-id> 1

Do not claim that installation succeeded without checking the command result. If the APK cannot be installed or the device is unavailable, document the exact blocker and leave the relevant gate incomplete.

Phase-specific smoke tests

- UI phase: navigation, scrolling, layout, back behavior, accessibility.
- Authentication phase: login, logout, session expiry, protected requests.
- Video phase: upload, processing, playback, seeking, retry.
- Social phase: likes, comments, follow, block, notifications.
- Admin phase: permissions, configuration publishing, audit records.
- Analytics phase: event ingestion, deduplication, dashboard accuracy.
- Rewards phase: eligibility, budget limits, ledger, withdrawal states.
- Release phase: clean installation, upgrade installation, network failures, crash checks.

17. GIT AND CHANGE MANAGEMENT

- Work on a feature branch when practical.
- Make small, meaningful commits.
- Never commit secrets, signing keys, raw credentials, or production database dumps.
- Do not force-push or delete important branches without explicit authorization.
- Review diffs before committing.
- Do not overwrite existing work without inspecting it.
- Keep database migrations immutable after deployment; use new corrective migrations.
- Preserve a reproducible build process.
- Produce an APK artifact for each accepted phase.

18. REQUIRED REPORT AFTER EVERY PHASE

Provide a concise, factual report containing:

1. Phase and objective
2. Repository files added or changed
3. Features actually implemented
4. Database migrations applied
5. APIs implemented
6. Automated tests executed and their actual results
7. APK build status
8. APK artifact path if verified
9. Physical-device installation status
10. Manual device tests performed and results
11. Known bugs, blockers, and security concerns
12. Git commit hash
13. Next phase and acceptance criteria

Use explicit statuses:

- PASS
- FAIL
- BLOCKED
- NOT TESTED

Never report a BLOCKED or NOT TESTED task as complete.

19. HOW TO START

Begin with Phase 0 only.

Inspect the actual repository and report:

- Current architecture
- Existing working functionality
- Missing functionality
- Current build status
- Current APK location if verified
- Important technical risks
- The smallest useful next implementation phase

Do not implement all phases at once.

After the audit, propose the exact Phase 1 scope and its acceptance tests. Proceed incrementally, preserving a working APK throughout development.

The final goal is not merely to produce an APK that installs. The goal is to build a secure, maintainable, testable, production-oriented Flikzo platform with a polished Android experience, a real backend, a reliable database, a comprehensive admin panel, and a carefully controlled creator-reward system.
