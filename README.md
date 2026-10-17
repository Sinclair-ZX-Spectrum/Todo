# Daymark — Smart To-Do List

A polished, responsive single-page to-do app with task management, productivity metrics, an Outlook-inspired notification center, reminder preferences, dark mode, and browser desktop notifications.

## Run it

1. Extract `smart-todo-outlook.zip`.
2. Open `index.html` in a current desktop browser. For best browser-notification support, serve the folder from localhost:
   - Python: `python3 -m http.server 8000`
   - Open `http://localhost:8000`
3. Tasks, notification history, and preferences are stored in this browser's `localStorage`.

No build tools or external dependencies are required.

## Features

- Create, edit, complete, and delete tasks with descriptions, due date/time, category, priority, and reminder intervals.
- Search, filter by status/priority, and sort tasks.
- Dashboard totals, overdue/today indicators, and completion percentage.
- Notification bell with unread count, read/unread actions, dismiss, and clear-history actions.
- Reminder intervals (1 day, 1 hour, 30 minutes, 10 minutes), duplicate suppression, overdue reminders, and cancellation by completion/deletion/rescheduling.
- Browser desktop notifications after the user explicitly enables them and grants permission.
- Quiet hours, high-priority-only desktop alerts, optional sound, time zone display, and light/dark themes.
- Export a JSON backup of tasks, notifications, and settings.

## Notification reliability — important

This is a **single-browser prototype**, not a hosted push-notification service.

- **While the page is open:** reminders are checked every 30 seconds; in-app notifications are saved in local storage. Desktop notifications can appear if supported, enabled, and permission is granted.
- **In a background tab/minimized browser:** the browser may throttle timers. Desktop notifications work when the reminder check runs, but timing is not guaranteed.
- **When the browser is fully closed:** this version cannot reliably run reminder checks or deliver notifications.
- **Reliable closed-browser delivery:** requires a backend scheduler, persisted user accounts/tasks, Web Push subscriptions, a push service, and a service worker. The backend would need to check deadlines and send pushes even when the browser is closed. That infrastructure is intentionally not faked in this local prototype.
- Browser notification permission is requested only when the user turns on **Desktop notifications** in Settings.
- Reminder delivery cannot be guaranteed by browser APIs; browser permission, operating-system settings, power management, and background throttling can affect delivery.

## Data and privacy

Data remains in local storage on the current browser profile/device. Clearing site data or using another browser/device will not carry tasks over. Use **Export tasks & settings** for a backup. This demo has no server, login, cloud sync, or multi-user database.

## Suggested production upgrades

For real multi-device and closed-browser reminders: add a backend (for example Node.js + PostgreSQL), authenticated accounts, a scheduled job/queue, VAPID Web Push subscriptions, a service worker, and server-side duplicate keys per task/reminder interval. Store due times as UTC instants while retaining the user's IANA time-zone identifier, and test DST transitions, rescheduling, revocation, retries, and expired push subscriptions.
