# Hedon and Health Simulation

This Python code simulates the accumulation of "hedons" (a measure of enjoyment) and health points based on a series of activities performed by a user: running, carrying textbooks, and resting. The simulation also incorporates a "star" bonus mechanic. This is a project that is part of ESC180 (Introduction to Computer Programming) at the University of Toronto

## Description

The code defines several functions to manage the simulation:

*   `initialize()`: Initializes all global variables, setting initial values for hedons, health, time, and activity tracking.
*   `star_can_be_taken(activity)`: Checks if a "star" bonus can be awarded for a given activity based on recent activity and star history.
*   `offer_star(activity)`: Awards a "star" bonus for a given activity, updating star tracking variables and potentially setting the user as "bored with stars" if they receive too many in a short period.
*   `perform_activity(activity, duration)`: Simulates performing an activity for a given duration, updating hedons, health, and time accordingly. This function contains the core logic for calculating hedons and health gains/losses.
*   `get_cur_hedons()`: Returns the current hedons.
*   `get_cur_health()`: Returns the current health.
*   `most_fun_activity_minute()`: Determines which activity would provide the most hedons per minute if performed for one minute. It uses temporary variables to simulate each activity without affecting the actual simulation state.

## Global Variables

The code uses several global variables to track the simulation state:

*   `cur_hedons`: Current number of hedons.
*   `cur_health`: Current health points.
*   `last_star_time`, `last_star_time2`, `last_star_time3`: Timestamps of the last three star bonuses.
*   `cur_time`: Current simulation time.
*   `last_activity`: The last activity performed.
*   `last_activity_duration`: The duration of the last activity.
*   `last_running_time`: Timestamp of the last running activity.
*   `last_textbooks_time`: Timestamp of the last textbooks activity.
*   `last_resting_time`: Timestamp of the last resting activity.
*   `last_running_duration`: The duration of the last running activity.
*   `cur_star_activity`: The activity associated with the current star bonus.
*   `last_finished`: Timestamp of the last activity finished.
*   `bored_with_stars`: Boolean indicating if the user is bored with stars.
*   `stars`: Number of consecutive stars received for the same activity.

## Usage

1.  **Run the Python script.**
2.  **Call `initialize()`:** This *must* be called before performing any activities.
3.  **Call `perform_activity(activity, duration)`:** Simulate performing an activity.
4.  **Call `get_cur_hedons()` and `get_cur_health()`:** Retrieve the current hedons and health.
5.  **Call `most_fun_activity_minute()`:** Determine the most hedonistic activity for one minute.

## Activity and Star Bonus Logic (OG Rules Implemented)

The `perform_activity` function implements the following rules:

*   **Initial State:** The user starts with 0 health and 0 hedons.
*   **Activity Options:** The user can perform one of three activities: running, carrying textbooks, or resting.
*   **Running:**
    *   **Health Gain:** 3 health points per minute for the first 180 minutes of *continuous* running. After 180 minutes of continuous running, the health gain is 1 point per minute. *Importantly, breaks in running (e.g., resting or carrying textbooks) reset the 180-minute threshold.*
    *   **Hedon Gain/Loss (No Star):**
        *   If the user is *not tired* (last activity finished more than 2 hours ago): 2 hedons per minute for the first 10 minutes, -2 hedons per minute after that.
        *   If the user *is tired* (last activity finished less than 2 hours ago): -2 hedons per minute.
*   **Carrying Textbooks:**
    *   **Health Gain:** 2 health points per minute.
    *   **Hedon Gain/Loss (No Star):**
        *   If the user is *not tired*: 1 hedon per minute for the first 20 minutes, -1 hedon per minute after that.
        *   If the user *is tired*: -2 hedons per minute.
*   **Resting:**
    *   **Hedon Gain:** 0 hedons per minute.
    *   **Health Gain:** 0 health per minute.
*   **Star Bonus:**
    *   If a star is offered for an activity and the user performs that activity *immediately* after the star is offered, they gain an *additional* 3 hedons per minute for up to 10 minutes.
    *   The star bonus only applies to the *first* call to `perform_activity` immediately following the `offer_star` call. Subsequent calls to `perform_activity` for the same activity do *not* receive the bonus, even if within the 10-minute window.
    *   **Star Boredom:** If three stars are offered within a 2-hour period, the user becomes "bored with stars," and no further star bonuses are awarded for the rest of the simulation.

## Helper Functions (Recommended)

The placeholder functions (`get_effective_minutes_left_hedons`, `get_effective_minutes_left_health`, `estimate_hedons_delta`, `estimate_health_delta`) are strongly recommended to be implemented to improve code organization and readability.
