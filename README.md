# ForWatchOS

WatchOS app are written on framework called WatchKit.
Code run on watch but apple watch is tightly linked to iPhone, writing apps for apple watch also means writing an iOS app.

How user interacts with apple watch: 4 different components 
1. fulls apps
> multiple screens, range of interactions
2. glances // NOT ANYMORE, use dock
> single screen content that can be accessed by swiping up the watch face. NO INTERACTION, ONLY DISPLAY. if user taps, app lauches.
3. notification
> Notifications appear when watchOS app's counterpart iOS app receives notification.
4. complications
> small elements that are embedded into certain watch faces.
Not interactive, but lets app add more information to to most quickly accessible part of phone' interface. They can also participate in Time Travel.
5. dock
> Let you hop on multiple app easily - recently opened OR favourites 

Face / Apps - Crown button
Dock - Side/Power button
Time Travel / Info from future - Spin crown

Feature:
Handoff - lets you start something on one devide & pick you on other device.
Return to clock - watch will return to watch face after.
Walkie Takie

CLOCK KIT
- framework to implement complications for you app. To display app specfic information on clock face.

WATCH KIT 
- Provides infrastructure for creating watchOS App.

All watchOS app must have "Extension" Delegate to 
- respond to app's life cycle event
- manage background task
- extend runtime sessions
- siri intent
- workout sessions 
- hand off activity

#App State

![alt text](https://docs-assets.developer.apple.com/published/74077a8107/ec07a686-2315-4700-9415-6485cc3bcfff.png)

Not Running 
- app not started OR system suspended & purged app

Inactive 
- Running in foreground [code runs], receive no action from control or gesture
- Newly launched app stays briefly to transition in Active state
- Active app transition to this state to prepare for background state

Active
- Code Running, receives action

Background
- System given small time to execute background task or session before suspending app.

Suspended
- App in memory but not executing code.
- System may purge suspended app to make room for other app, relauches app as soonn as available.

#Extension delegate lifecycle method's

applicationDidFinishLaunching()
Not Running to Inactive OR background

applicationDidBecameActive(), ApplicationWillResignActive()
Inactive - Active

applicationWill/DidEnterBackground
Inactive to Background

Except for applicationDidFinishLaunching(), system calls extension delegate methods only for main interface.

For Notification, use navigation controller's method to track state
will/didActivate()

#Interface State

Notification interface is active when displaying a dynamic notification.

App's Snapshot 
Use snapshot background task to show timely & accurate snapshot of your app
App State - Background, Interface State - Active, but not shown.

State Transitions

App Launch
1. WKApplicationState.inactive, System calls extension delegate's applicationDidFinishLaunching()
2. System instantiate intial interface controller, call its awake(with Context)
3. WKApplicationState.active, System calls applicationDidBecomeActive()
4. System calls initial interface controller's willActivate()
5. App appears on screen, System calls initial interface controller's didAppear()

Background
- lower arm/screen turn off 
- cover screen/press crown button, does not become inactive or frontmostApp, goes to background quickly.

1. system call applicationWillResignActive()
2. App transition to WKApplicationState.inactive, stay for 2 minutes[by default] as it is frontmost app
3. WKApplicationState.background, applicationDidEnterBackground()
4. system calls presented interface controller's didDeactivate()
5. System suspends app

Reactivate app. eg. tap complication on active watch face

1. if suspended but in memory, WKApplicationState.background
2. system call "applicationWillEnterForeground()"
3. WKApplicationState.active, applicationDidBecomeActive()
4. system call initial interface controller's willActivate()


Frontmost Application
- app thats receives key events
- app running, user drops arm or screen off
- App transition active to inactive state, but not background state.
- App stay in frontmost App state for 2 min [by default], can extend isFrontmostTimeoutExtended
- if user cover screen/press crown button, does not become inactive or frontmostApp, goes to background quickly.
Keeps feature:
1. haptic feedback 2. notifications 3. Respond to background URL task

Receive Background Data:
When system receive background data, it may not immediately wake up app [take within 10 minutes]. Instead delay to preserve battery.












