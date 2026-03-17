# OpenCPT - Open Source Continuous Performance Test

**A tool for measuring and tracking sustained attention over time**

## Overview

There's growing concern that social media, short-form video, and constant digital stimulation may be affecting our ability to sustain attention. While OpenCPT cannot diagnose medical conditions, it provides a way to track your own focus abilities over time and observe how lifestyle changes might affect your concentration.

OpenCPT could be used to monitor your sustained attention abilities over weeks, months, or years. The longitudinal data can help you understand whether interventions like meditation, exercise, or reduced screen time are having measurable effects on your ability to focus.

## Design Philosophy

OpenCPT is based on the AX-CPT (Continuous Performance Test) paradigm, a research tool used in psychology and neuroscience to measure sustained attention, inhibitory control, and context maintenance. The AX-CPT variant requires participants to respond only in specific contexts (when X appears after A), testing both the ability to maintain focus and to inhibit automatic responses. This implementation follows established scientific protocols while remaining accessible for personal use.

As an open-source project, OpenCPT provides complete transparency in its methodology—anyone can audit the code, verify the approach, and contribute improvements.

All data is stored locally on your machine. There is no cloud synchronization, no telemetry, and no external data collection. You maintain complete ownership and control of your cognitive performance data, which can include sensitive behavioral information.

The addition of optional webcam-based behavioral tracking extends measurement beyond simple button presses. By capturing blinks, head movement, and posture, OpenCPT can identify attention lapses, fatigue patterns, and restlessness that may not be apparent in response data alone. These objective behavioral markers provide richer insight into sustained attention performance.

The task is intentionally monotonous—watching letters appear for extended periods is not entertaining, but that is what effectively tests the ability to maintain focus when stimuli become repetitive.

## Use Cases

- **Personal longitudinal tracking**: Monitor your attention abilities over months or years to detect changes
- **Self-experimentation**: Test how sleep, caffeine, exercise, meditation, or other lifestyle factors affect your focus
- **Research & Education**: Use in psychology courses, personal research projects, or cognitive studies
- **Mindfulness practice**: Use as a focus training tool or meditation exercise

## ⚠️ Important Disclaimers

- **NOT a medical diagnostic tool** - This is for personal experimentation and research only
- **NOT validated for clinical use** - Do not use this to diagnose any medical or psychological condition
- **Results are for personal tracking** - Trends over time are more meaningful than single sessions
- **Privacy-focused** - All data stays on your computer; nothing is uploaded anywhere

## Features

- ✅ Scientifically-based task design (AX-CPT paradigm used in research)
- ✅ Optional webcam-based behavioral tracking (blinks, head movement, posture)
- ✅ Comprehensive data export (CSV format for analysis)
- ✅ Jupyter notebook for visualization and analysis
- ✅ Fully configurable (duration, difficulty, display settings)
- ✅ Privacy-focused (all data stored locally)
- ✅ Open source (modify and extend as you wish)

## Task Description

The task is simple in concept but challenging in execution:

1. Letters appear on screen one at a time (A, B, X, or Y)
2. **Press SPACEBAR only when you see X immediately after A**
3. Do not respond to any other letter combinations
4. The task runs for the configured duration (default: 20 minutes)

**Trial Types:**
- **AX**: A followed by X → **RESPOND** (target trial)
- **BX**: B followed by X → Do not respond (tests inhibitory control)
- **AY**: A followed by Y → Do not respond (tests context maintenance)
- **BY**: B followed by Y → Do not respond (baseline)

The challenge lies in maintaining focus and context over hundreds of trials while resisting the urge to respond to X when it appears outside the correct context.

**Measured Variables:**
- Response accuracy (correct vs. incorrect responses)
- Reaction times (response latency)
- Error patterns (performance across different trial types)
- Behavioral markers (blinks, head movement, posture changes - if webcam tracking enabled)

## Getting Started

### Requirements

- Python 3.7+
- Pygame
- OpenCV (for webcam tracking)
- MediaPipe (for face/eye tracking)
- NumPy

### Installation

Install dependencies:
```bash
pip install -r requirements.txt
```

Or manually:
```bash
pip install pygame opencv-python mediapipe numpy
```

### Running the Task

```bash
python main.py
```

Before the task begins, you will be prompted to answer several questions (age, caffeine intake, sleep quality, etc.) to facilitate correlation analysis in your longitudinal data.

### Controls

- **SPACEBAR** (or configured key): Respond when you see X after A
- **ESC**: Quit task early (data will still be saved)

## Configuration

Edit `config.json` to customize task parameters:

### Timing Parameters
- `stimulus_duration_ms`: How long each letter appears (default: 50ms)
- `response_window_ms`: Time allowed to respond after stimulus (default: 500ms)
- `inter_stimulus_interval_ms`: Delay between stimuli (default: 50ms)

### Task Parameters
- `session_duration_minutes`: Total session duration in minutes (default: 1 minute for testing, recommend 20+ for actual use)
- `target_probability`: Frequency of AX trials (default: 0.25 = 25%)
- `practice_trials`: Number of practice trials before the main task (default: 20)

**Note**: The total number of trials is automatically calculated based on `session_duration_minutes` and the timing parameters. Each trial pair (cue + probe) takes: `(stimulus_duration_ms + response_window_ms + inter_stimulus_interval_ms) × 2`

### Display Parameters
- `fullscreen`: Run in fullscreen mode (default: true)
- `font_size`: Size of stimulus letters (default: 96)
- `background_color`: RGB color for background (default: [0, 0, 0] = black)
- `stimulus_color`: RGB color for stimuli (default: [255, 255, 255] = white)
- `random_colors`: Randomly vary stimulus colors each trial (default: false)

### Fixation & Input
- `show_fixation`: Show fixation cross during ISI (default: false)
- `fixation_symbol`: Symbol to show during ISI (default: "+")
- `response_key`: Key to press for responses (default: "space")

### Webcam Tracking
- `webcam_tracking.enabled`: Enable/disable webcam tracking (default: true)
- `webcam_tracking.camera_index`: Camera device index (default: 0 for default webcam)
- `webcam_tracking.capture_fps`: Webcam capture frame rate (default: 30)
- `webcam_tracking.jpeg_quality`: JPEG compression quality for saved frames (default: 85)
- `webcam_tracking.save_frame_data`: Save frame-by-frame tracking data (default: true)
- `webcam_tracking.save_session_summary`: Save session-level summary (default: true)

## Data Output

Results are automatically saved to CSV files with timestamps in the `results/` directory.

### Data Hierarchy

**Understanding the three levels of data:**

- **FRAME**: Single webcam capture (30 FPS by default)
  - Contains: Head position, eye state at one moment in time
  - Example: At 12.345 seconds, head was at position (0.5, 0.2, 2.8), eyes open

- **TRIAL**: One cue-probe pair (duration depends on timing configuration)
  - Contains: Aggregated metrics from all frames in that trial
  - Example: During trial #42, participant blinked 3 times, head moved 0.15 units
  - With default timing (50ms stimulus + 500ms response + 50ms ISI × 2), each trial pair takes ~1.2 seconds

- **SESSION**: Complete test run (configurable duration)
  - Contains: Overall statistics aggregated from all trials
  - Example: Across entire session, average blink rate was 36/min, posture consistency 0.83
  - Number of trials depends on session duration and timing parameters

### Main Trial Data
File: `results/TIMESTAMP_DURATION/trial_data.csv`

One row per trial (cue-probe pair). Number of rows depends on session duration and timing configuration.

**Behavioral Columns:**
- `trial_index`: Trial number (one trial = one cue-probe pair)
- `stimulus`: Letter shown (A, B, X, or Y)
- `previous_stimulus`: Previous letter shown
- `trial_type`: AX, BX, AY, BY, or NONE
- `response`: 1 if participant responded, 0 if not
- `correct`: 1 if response was correct, 0 if incorrect
- `reaction_time_ms`: Response time in milliseconds (empty if no response)
- `stimulus_onset_timestamp`: High-precision timestamp of stimulus onset

**Tracking Columns (if enabled, aggregated from all frames in the trial):**
- `blink_count`: Number of blinks during this trial
- `blink_rate`: Blinks per second during this trial
- `mean_head_distance`: Average distance from camera during this trial
- `head_movement_variance`: Head position variance during this trial (lower = more stable)
- `looking_away_count`: Number of frames where head turned >30° away
- `frames_tracked`: Number of frames successfully tracked in this trial

### Frame-Level Tracking Data (if enabled)
File: `results/TIMESTAMP_DURATION/tracking_frames.csv`

High-frequency data (30 FPS by default, configurable) for detailed analysis. Each row = one webcam frame.

- `timestamp`: Frame timestamp
- `trial_index`: Which trial this frame belongs to
- `head_x`, `head_y`, `head_z`: Head position at this moment (z = distance from camera)
- `head_pitch`, `head_yaw`, `head_roll`: Head orientation at this moment (degrees)
- `left_eye_aspect_ratio`, `right_eye_aspect_ratio`: Eye openness at this moment (lower = more closed)
- `is_blinking`: Whether eyes were closed at this moment

### Session Summary (if enabled)
File: `results/TIMESTAMP_DURATION/tracking_session.csv`

Aggregate metrics for the entire session. One row total, aggregated from all trials.

- `total_blinks`: Total blinks across all trials
- `total_frames_tracked`: Total frames processed
- `total_trials_tracked`: Number of trials with tracking data
- `blink_rate_per_minute`: Average blinks per minute across entire session
- `mean_head_movement`: Overall head movement across entire session (lower = more stable)
- `posture_consistency`: Posture consistency metric (0-1, higher = better consistency)
- `fatigue_indicator`: Change in blink rate over time (positive = increased fatigue)
- `session_duration_seconds`: Total tracking duration

## Analyzing Results

### Jupyter Notebook Analysis

A comprehensive Jupyter notebook is provided for analyzing your results:

```bash
# Install analysis dependencies (if not already installed)
pip install pandas matplotlib seaborn jupyter

# Launch Jupyter
jupyter notebook analyze_axcpt_results.ipynb
```

The notebook includes:
- **Behavioral Performance Analysis**: Accuracy, reaction times, error patterns
- **Tracking Data Visualization**: Blink rates, head movement, engagement metrics
- **Combined Analysis**: Correlation between attention and performance
- **Trial-by-Trial Deep Dive**: Detailed inspection of individual trials
- **Professional Visualizations**: Publication-ready charts and graphs

The notebook automatically loads the most recent session data from the `results/` folder.

## Understanding Your Results

### Interpreting Performance Metrics

**Accuracy**: Overall correctness across all trials. Higher values indicate better performance, though even experienced participants rarely achieve 100% accuracy.

**Reaction Time**: Response latency on target trials (AX). Faster reaction times generally indicate better sustained attention, though consistency is more informative than raw speed.

**Error Patterns**:
- **False alarms on BX trials**: Difficulty with inhibitory control (automatic responding to X regardless of context)
- **Misses on AX trials**: Attention lapses or fatigue
- **False alarms on AY trials**: Difficulty maintaining context information (failure to remember the previous letter)

**Behavioral Metrics** (if webcam tracking enabled):
- **Blink rate**: Typically increases with fatigue and mental effort
- **Head movement**: Increased movement may indicate restlessness or discomfort
- **Looking away**: Direct measure of attention lapses

### Longitudinal Analysis

Single sessions contain considerable noise—performance can vary due to numerous factors. The primary value of OpenCPT lies in tracking trends over weeks or months:

- **Improving accuracy over time**: Practice effects or genuine improvement in sustained attention
- **Stable performance**: Consistent baseline attention abilities
- **Declining performance**: May indicate fatigue, stress, or other factors warranting investigation
- **Correlation with lifestyle factors**: Compare performance data with sleep, exercise, caffeine intake, etc. (tracked in pre-test questionnaire)

### Privacy & Data Ownership

- **All data stays local**: Nothing is uploaded to any server or cloud service
- **You own your data**: All CSV files are in the `results/` folder—analyze, share, or delete as you wish
- **No telemetry**: The application doesn't phone home or track usage
- **No account required**: No sign-up, no tracking, no data collection

## Webcam Tracking Features

The task now includes **optional webcam-based behavioral tracking** using MediaPipe for real-time face and eye detection.

### What's Tracked (Tier 1 - Currently Implemented)

**Blink Detection:**
- Automatic blink detection using Eye Aspect Ratio (EAR)
- Blink count and rate per trial
- Can identify fatigue patterns over time

**Head Pose Tracking:**
- 3D head position (X, Y, Z coordinates)
- Head orientation (pitch, yaw, roll in degrees)
- Head movement stability/variance
- Detection of looking away from screen

### Research Applications

This data enables analysis of:
- **Attention lapses**: Correlation between looking away and missed targets
- **Fatigue**: Increased blink rate over time
- **Posture Consistency**: Head movement and posture changes
- **Restlessness**: Head movement patterns during sustained attention tasks
- **Error prediction**: Behavioral markers before incorrect responses

### Testing Tracking

Test the tracking system without running the full experiment:
```bash
python test_tracking.py
```

This will run a 15-second test (3 trials × 5 seconds) and save test data files.

### Disabling Tracking

To run the task without webcam tracking, set in `config.json`:
```json
"webcam_tracking": {
  "enabled": false
}
```

### Future Enhancements (Tier 2/3)

The architecture is designed to easily add:
- **Blink duration**: Measure how long each blink lasts (longer blinks may indicate fatigue)
- **PERCLOS (Percentage of Eye Closure)**: Percentage of time eyes are >80% closed (drowsiness indicator)
- **Gaze tracking**: Where on screen the participant is looking
- **Pupil dilation**: Cognitive load and arousal measurement
- **Facial expressions**: Emotion detection (frustration, boredom)
- **Advanced attention metrics**: Fixation stability, saccade detection

## Project Structure

```
/ax-cpt
  ├── main.py                    # Main game loop and rendering
  ├── config.json                # Configuration parameters
  ├── stimulus_generator.py      # Trial sequence generation
  ├── logger.py                  # CSV data logging (behavioral data)
  ├── utils.py                   # Timing and utility functions
  ├── webcam_tracker.py          # Webcam tracking with MediaPipe
  ├── tracking_logger.py         # Multi-level tracking data logger
  ├── questionnaire.py           # Pre-test questionnaire for metadata
  ├── session_metadata.py        # Session metadata management
  ├── summary_report.py          # ASCII summary report generator
  ├── requirements.txt           # Python dependencies
  ├── README.md                  # This file
  ├── tests/                     # Test scripts
  │   ├── test_tracking.py       # Test script for tracking system
  │   ├── test_summary.py        # Test script for summary reports
  │   └── test_generator.py      # Test script for stimulus generation
  └── results/                   # Session data (timestamped folders)
      └── YYYY-MM-DDTHH-MM-SS_XXm/
          ├── config.json        # Session configuration snapshot
          ├── trial_data.csv     # Behavioral performance data
          ├── tracking_frames.csv      # Frame-level tracking data
          ├── tracking_session.csv     # Session-level tracking summary
          ├── session_metadata.csv     # Pre-test questionnaire responses
          └── summary.txt        # Human-readable summary report
```

## Technical Notes

- The task uses high-precision timing (`time.perf_counter()`)
- Frame-locked rendering at 60 FPS
- No feedback is provided during trials
- First trial cannot be AX (no prior context)
- Trial sequences are randomized while maintaining target probability

## Contributing

This is an open-source project! Contributions are welcome:

- **Bug reports**: Open an issue if you find problems
- **Feature requests**: Suggest improvements or new tracking metrics
- **Code contributions**: Submit pull requests
- **Documentation**: Help improve these docs
- **Research**: Share interesting findings from your data (anonymized, of course)


## Acknowledgments

This implementation is based on the AX-CPT paradigm used in cognitive neuroscience research. The task design follows established protocols for measuring sustained attention and cognitive control.

---

**Remember**: This is a tool for personal experimentation and tracking. Trends over time are more meaningful than individual sessions. Be patient with yourself, and use the data to understand your attention patterns, not to judge yourself.

