# FleetClok 🚛

> A full-stack mobile app for managing employees — built with React Native, Expo, and Supabase.

📱 **[Watch Demo](https://youtu.be/7G-LxvrmjbU)** &nbsp;·&nbsp; 🔒 Source code is private

---

## Overview

FleetClok is a production-ready workforce management app built for a trucking/fleet operation. It handles everything from employee clock-in/out with GPS enforcement to payroll calculation and fuel tracking — all in real time.

---

## Features

### 👷 Employee Side
- **Clock In / Out** with GPS geofence enforcement — employees must be physically at the yard
- **Unit & Odometer logging** at start and end of every shift
- **Personal timesheet** — view hours worked by week
- **Fuel reporting** — log fuel stops with date, unit, odometer, and litres
- **Same-day edits** — employees can correct or delete today's fuel entries

### 🔐 Admin Login Approval
- Employees cannot log in without admin approval
- When an employee attempts login, admin instantly receives a **push notification** with the one-time code — even if the phone is locked
- Code expires in 60 seconds

### 📋 Admin — Time Tracking
- Live view of all currently clocked-in drivers (independent of selected week)
- Search by employee name or unit number
- Approve timesheet entries for payroll
- Export to **Excel**

### 💰 Admin — Payroll
- Calculate gross pay, HST, and net pay per driver for any period
- Per-driver hourly rates
- Export to **Excel**

### ⛽ Admin — Fuel Sheet
- View all fuel reports grouped by unit for any week
- Filter by specific unit
- Export to **Excel**

### 🗺️ Admin — Other
- **Yard Location** — set geofence center point and radius on a map
- **Vehicle Log** — review odometer in/out per unit per shift
- **Employee Management** — invite, activate, and deactivate employees

---

## Tech Stack

| | |
|---|---|
| **Framework** | React Native + Expo SDK 54 |
| **Backend** | Supabase (PostgreSQL, Auth, Realtime) |
| **Serverless** | Supabase Edge Functions (Deno) |
| **Push Notifications** | Expo Notifications + Expo Push Service |
| **Location** | expo-location (GPS geofencing) |
| **Security** | Row Level Security, encrypted session tokens, OTP expiry |
| **Export** | SheetJS — Excel (.xlsx) generation on-device |
| **Navigation** | React Navigation (Stack) |

---

## Architecture Highlights

**Single-device enforcement** — logging in on a new device automatically signs out the previous one via Supabase Realtime and rotating session tokens stored in secure enclave.

**Real-time admin alerts** — a Supabase Database Webhook triggers an Edge Function the moment an employee requests login, sending a push notification to the admin's device via Expo's push API — no polling required.

**Geofence clock-in** — uses device GPS and Haversine distance formula to verify the employee is within the configured yard radius before allowing clock in or out.

**Excel export** — timesheets, payroll, and fuel reports are exported as native `.xlsx` files generated on-device using SheetJS, then shared via the native OS share sheet.

---

## Security

- Supabase credentials injected at build time via environment variables — never hardcoded
- Row Level Security (RLS) on all tables — employees can only read and write their own data
- OTP codes expire after 60 seconds and are single-use
- Session tokens prevent concurrent logins across multiple devices

---
