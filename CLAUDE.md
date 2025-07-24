# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **Turtle Run** design repository - a Korean running-based territory conquest game concept. This repository contains design specifications, user interface prototypes, and documentation for a mobile game where users claim territory (called "Shell") through running activities.

**Key Concept**: Users run to create polygon-shaped territories called "Shell" based on their GPS routes. The game features a species system (turtle types) that provide different buffs to enhance Shell area calculations.

## Project Structure

- **README.md**: Complete Korean specification document with technical stack, features, and implementation details
- **user_scenarios.md**: Detailed user flow scenarios in Korean
- **prototype/**: HTML/CSS prototypes for mobile UI screens
- **screenshots/**: iPhone XR mockups of all prototype screens

## Design System

### Color Palette
The project uses a dark green urban theme:
- Main: `#1A3A2F` (dark green)
- Secondary: `#2C4F43` (medium green)  
- Accent: `#4A9D7F` (bright green)
- Background: `#121212` (dark)
- Text: `#FFFFFF` (white)

### Species Colors
- Red-eared Slider (붉은귀거북): `#E53E3E` (red)
- Desert Tortoise (사막거북): `#D69E2E` (gold)
- Greek Tortoise (그리스거북): `#3182CE` (blue)

### Rank Colors
- Hatchling: `#718096` (gray)
- Juvenile: `#CD7F32` (bronze)
- Subadult: `#C0C0C0` (silver)
- Adult: `#FFD700` (gold)
- Mature: `#E5E4E2` (platinum)
- Elder: `#B9F2FF` (diamond)
- Legend: `#9F7AEA` (purple)

## Core Game Mechanics

### Shell System
- Users create polygon territories through running GPS routes
- Shell size calculated from area with species-specific buffs
- Shell area converts 1:1 to Shell Points (SP)

### Species System (3 turtle types)
Each provides 100% total buff potential:
1. **Red-eared Slider**: Speed-focused (fast pace emphasis)
2. **Desert Tortoise**: Endurance-focused (long distance emphasis)  
3. **Greek Tortoise**: Explorer-focused (new route discovery bonus)

### Ranking System
7-tier SP-based ranking from Hatchling (0-999 SP) to Legend (250,000+ SP)

## Development Context

### Planned Technical Stack
- **Frontend**: iOS Native (Swift/SwiftUI)
- **Backend**: Java Spring Boot
- **Database**: PostgreSQL with PostGIS
- **Features**: GPS tracking, HealthKit integration, real-time mapping

### Development Phase
This repository contains design and prototyping work for a mobile game concept. No actual application code exists - only specifications, user flows, and HTML/CSS UI prototypes for visualization.

## Working with This Repository

Since this is a design repository:
- Prototype files are static HTML with embedded CSS
- No build system or package management
- Focus on design consistency and user experience flow
- All documentation is in Korean
- Mobile-first design approach (iPhone XR references)

## Key Files for Understanding

1. `README.md:6-17` - Core app concept and platform details
2. `README.md:56-78` - Species buff system mechanics  
3. `README.md:95-107` - SP and ranking system
4. `user_scenarios.md:1-83` - Complete user onboarding flow
5. `prototype/1.1.1.login.html:8-15` - CSS variables for color system