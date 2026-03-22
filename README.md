# OpenCPT - Open Source Continuous Performance Test

**Real-time computer vision and behavioral data pipeline for cognitive performance tracking**

## Overview

OpenCPT is a machine learning-powered cognitive assessment platform that combines real-time computer vision, multi-level data aggregation, and longitudinal analytics to measure sustained attention and cognitive performance. The system integrates MediaPipe's face mesh detection models with a custom behavioral tracking pipeline to capture, process, and analyze cognitive performance data at scale.

**Key Technical Features:**
- **Real-time ML inference**: MediaPipe face mesh detection running at 30 FPS for behavioral tracking
- **Multi-level data pipeline**: Frame-level (30 Hz) → Trial-level → Session-level aggregation with statistical feature engineering
- **Time-series analysis**: Longitudinal tracking with correlation analysis across behavioral and performance metrics
- **Scalable data architecture**: Hierarchical CSV storage optimized for both granular analysis and aggregate reporting

The platform enables tracking of sustained attention abilities over weeks, months, or years, with rich behavioral telemetry that goes beyond simple response metrics to include eye tracking, head pose estimation, and fatigue detection.

## Technology Stack

**Machine Learning & Computer Vision:**
- **MediaPipe** (Google): 468-point face mesh detection for real-time facial landmark tracking
- **OpenCV**: Image processing and camera I/O
- **NumPy**: Numerical computing for feature engineering and statistical calculations

**Data Processing & Analytics:**
- **Pandas**: Data manipulation and time-series analysis
- **Matplotlib/Seaborn**: Statistical visualization and plotting
- **Jupyter**: Interactive data exploration and analysis notebooks

**Application Framework:**
- **Pygame**: High-performance rendering and precise timing control
- **Python 3.7+**: Core application logic

## Technical Architecture

### Machine Learning Pipeline

**Computer Vision Models:**
- **MediaPipe Face Mesh**: 468-point facial landmark detection for real-time head pose estimation
- **Eye Aspect Ratio (EAR)**: Custom blink detection algorithm using geometric eye landmark ratios
- **3D Head Pose Estimation**: Pitch, yaw, roll calculation from facial landmarks with PnP solver

**Data Engineering:**
- **Multi-level aggregation pipeline**: Raw frame data (30 Hz) → Trial statistics → Session metrics
- **Feature engineering**: Variance-based movement detection, temporal blink rate calculation, posture consistency scoring
- **Streaming data processing**: Real-time frame capture with non-blocking I/O for minimal latency
- **Hierarchical storage**: Optimized CSV structure for both high-frequency analysis and summary reporting

### Scientific Foundation

OpenCPT implements the AX-CPT (Continuous Performance Test) paradigm, a validated research tool used in cognitive neuroscience to measure sustained attention, inhibitory control, and context maintenance. The task requires participants to respond only in specific contexts (when X appears after A), testing both the ability to maintain focus and to inhibit automatic responses.

The webcam-based behavioral tracking extends measurement beyond simple button presses. By capturing blinks, head movement, and posture through computer vision, the system identifies attention lapses, fatigue patterns, and restlessness that may not be apparent in response data alone. These objective behavioral markers provide richer insight into sustained attention performance.

### Privacy & Data Ownership

All data is stored locally on your machine. There is no cloud synchronization, no telemetry, and no external data collection. You maintain complete ownership and control of your cognitive performance data, which can include sensitive behavioral information. As an open-source project, OpenCPT provides complete transparency in its methodology—anyone can audit the code, verify the approach, and contribute improvements.

## Use Cases

- **Personal longitudinal tracking**: Monitor your attention abilities over months or years to detect changes
- **Self-experimentation**: Test how sleep, caffeine, exercise, meditation, or other lifestyle factors affect your focus
- **Research & Education**: Use in psychology courses, personal research projects, or cognitive studies
- **Mindfulness practice**: Use as a focus training tool or meditation exercise

## Important Disclaimers

- **NOT a medical diagnostic tool** - This is for personal experimentation and research only
- **NOT validated for clinical use** - Do not use this to diagnose any medical or psychological condition
- **Results are for personal tracking** - Trends over time are more meaningful than single sessions
- **Privacy-focused** - All data stays on your computer; nothing is uploaded anywhere

## Features

**Machine Learning & Computer Vision:**
- Real-time face mesh detection with MediaPipe (468 facial landmarks at 30 FPS)
- Custom blink detection algorithm using Eye Aspect Ratio (EAR)
- 3D head pose estimation (pitch, yaw, roll) with PnP solver
- Automated fatigue detection through temporal blink rate analysis

**Data Engineering & Analytics:**
- Multi-level data aggregation pipeline (frame → trial → session)
- Feature engineering: movement variance, posture consistency, attention metrics
- Comprehensive data export (hierarchical CSV format optimized for analysis)
- Jupyter notebook with pandas/matplotlib/seaborn for visualization and statistical analysis
- Time-series correlation analysis for longitudinal tracking

**System Design:**
- Scientifically-based task design (AX-CPT paradigm used in cognitive research)
- High-precision timing with frame-locked rendering (60 FPS)
- Fully configurable parameters (duration, difficulty, display settings)
- Privacy-focused architecture (all data stored locally, no telemetry)
- Open source and extensible

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
- **ML-derived behavioral features** (if webcam tracking enabled):
  - Blink count and temporal blink rate (fatigue indicator)
  - 3D head position and movement variance (attention stability)
  - Head pose angles (pitch, yaw, roll) for gaze estimation
  - Posture consistency metrics across trials

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

## Data Pipeline & Output

### Multi-Level Data Aggregation Architecture

The system implements a hierarchical data pipeline that processes raw sensor data through multiple aggregation stages:

**FRAME LEVEL** (Raw sensor data - 30 Hz sampling rate)
- **Input**: MediaPipe face mesh detection output (468 landmarks per frame)
- **Processing**: Eye Aspect Ratio calculation, 3D head pose estimation via PnP solver
- **Output**: Timestamped feature vectors (head_x, head_y, head_z, pitch, yaw, roll, left_EAR, right_EAR, is_blinking)
- **Volume**: ~1,800 frames per minute, ~36,000 frames per 20-minute session
- **Use case**: High-frequency analysis, event detection, temporal pattern analysis

**TRIAL LEVEL** (Aggregated statistics - per cue-probe pair)
- **Input**: All frames within trial window (~1.2 seconds = ~36 frames)
- **Feature Engineering**:
  - Blink count and rate calculation
  - Head movement variance (stability metric)
  - Mean head distance from camera
  - Looking-away detection (head yaw/pitch thresholds)
- **Output**: Trial-level feature vector merged with behavioral response data
- **Volume**: ~1,000 trials per 20-minute session
- **Use case**: Trial-by-trial correlation analysis, error prediction

**SESSION LEVEL** (Summary statistics - entire test run)
- **Input**: All trial-level aggregates
- **Feature Engineering**:
  - Overall blink rate per minute
  - Posture consistency score (inverse of position variance)
  - Fatigue indicator (temporal trend in blink rate)
  - Mean head movement across session
- **Output**: Single-row summary with session-wide metrics
- **Volume**: One record per session
- **Use case**: Longitudinal tracking, session comparison, trend analysis

Results are automatically saved to CSV files with timestamps in the `results/` directory.

### Trial-Level Data (Behavioral + ML Features)
File: `results/TIMESTAMP_DURATION/trial_data.csv`

One row per trial (cue-probe pair). Combines behavioral responses with ML-derived features.

**Behavioral Response Features:**
- `trial_index`: Trial number (one trial = one cue-probe pair)
- `stimulus`: Letter shown (A, B, X, or Y)
- `previous_stimulus`: Previous letter shown
- `trial_type`: AX, BX, AY, BY, or NONE
- `response`: 1 if participant responded, 0 if not
- `correct`: 1 if response was correct, 0 if incorrect
- `reaction_time_ms`: Response time in milliseconds (empty if no response)
- `stimulus_onset_timestamp`: High-precision timestamp of stimulus onset

**ML-Derived Tracking Features (aggregated from ~36 frames per trial):**
- `blink_count`: Number of blinks detected during trial
- `blink_rate`: Blinks per second (temporal feature)
- `mean_head_distance`: Average z-coordinate from camera (depth estimation)
- `head_movement_variance`: Variance in 3D head position (stability metric)
- `looking_away_count`: Number of frames where head pose exceeded threshold (|yaw| > 30° or |pitch| > 30°)
- `frames_tracked`: Number of frames with successful face detection (data quality metric)

### Frame-Level Raw Data (High-Frequency Sensor Stream)
File: `results/TIMESTAMP_DURATION/tracking_frames.csv`

Raw computer vision output at 30 Hz sampling rate. Each row represents one processed frame from the webcam.

**Temporal Features:**
- `timestamp`: High-precision frame timestamp (seconds since session start)
- `trial_index`: Foreign key linking to trial data

**3D Head Pose Features (from MediaPipe face mesh + PnP solver):**
- `head_x`, `head_y`, `head_z`: 3D head position in camera coordinate system (z = depth/distance)
- `head_pitch`: Up/down rotation in degrees (nodding)
- `head_yaw`: Left/right rotation in degrees (head turning)
- `head_roll`: Tilt rotation in degrees (head tilting)

**Eye Tracking Features (from facial landmark geometry):**
- `left_eye_aspect_ratio`: Geometric ratio of eye height to width (lower = more closed)
- `right_eye_aspect_ratio`: Geometric ratio of eye height to width (lower = more closed)
- `is_blinking`: Binary classification (threshold-based on EAR < 0.2)

### Session-Level Summary Statistics
File: `results/TIMESTAMP_DURATION/tracking_session.csv`

Aggregate metrics computed across entire session. Single row containing session-wide statistics.

**Volume Metrics:**
- `total_blinks`: Sum of all detected blinks across session
- `total_frames_tracked`: Total frames with successful face detection
- `total_trials_tracked`: Number of trials with tracking data
- `session_duration_seconds`: Total tracking duration

**Engineered Performance Metrics:**
- `blink_rate_per_minute`: Mean blink frequency (normalized to per-minute rate)
- `mean_head_movement`: Average 3D position variance across session (stability indicator)
- `posture_consistency`: Inverse variance metric (0-1 scale, higher = more consistent posture)
- `fatigue_indicator`: Linear regression slope of blink rate over time (positive = increasing fatigue)

## Data Analysis & Visualization

### Jupyter Notebook Analytics Pipeline

A comprehensive Jupyter notebook (`analyze_axcpt_results.ipynb`) provides end-to-end data analysis using pandas, matplotlib, and seaborn:

```bash
# Install analysis dependencies (if not already installed)
pip install pandas matplotlib seaborn jupyter

# Launch Jupyter
jupyter notebook analyze_axcpt_results.ipynb
```

**Analysis Modules:**

1. **Data Loading & Preprocessing**
   - Automated CSV ingestion from hierarchical storage
   - Data validation and quality checks
   - Missing data handling and interpolation

2. **Behavioral Performance Analysis**
   - Accuracy metrics by trial type (AX, BX, AY, BY)
   - Reaction time distribution analysis
   - Error pattern classification and visualization

3. **ML Feature Analysis**
   - Time-series visualization of blink rates and head movement
   - Correlation matrices between behavioral and CV-derived features
   - Fatigue detection through temporal trend analysis

4. **Combined Multi-Modal Analysis**
   - Correlation between attention metrics (blinks, head pose) and performance (accuracy, RT)
   - Predictive modeling: Can behavioral features predict errors?
   - Trial-by-trial deep dive with synchronized behavioral and tracking data

5. **Statistical Visualization**
   - Distribution plots, box plots, violin plots for metric comparison
   - Time-series plots with trend lines and confidence intervals
   - Heatmaps for correlation analysis
   - Publication-ready figures with customizable styling

The notebook automatically loads the most recent session data from the `results/` folder and provides interactive exploration of all three data levels (frame, trial, session).

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

## Computer Vision Tracking System

### Real-Time ML Inference Pipeline

The system implements **optional webcam-based behavioral tracking** using Google's MediaPipe framework for real-time face mesh detection and feature extraction.

**Architecture:**
- **Model**: MediaPipe Face Mesh (468 facial landmarks)
- **Inference Rate**: 30 FPS on CPU (optimized for real-time performance)
- **Processing**: Non-blocking frame capture with threaded I/O
- **Latency**: <33ms per frame (suitable for real-time behavioral monitoring)

### Implemented Features (Tier 1)

**Blink Detection Algorithm:**
- **Method**: Eye Aspect Ratio (EAR) calculation from eye landmark geometry
- **Formula**: EAR = (||p2-p6|| + ||p3-p5||) / (2 * ||p1-p4||) where p1-p6 are eye landmarks
- **Threshold**: EAR < 0.2 classified as blink
- **Output**: Binary blink state + temporal blink rate calculation
- **Application**: Fatigue detection through temporal trend analysis

**3D Head Pose Estimation:**
- **Method**: Perspective-n-Point (PnP) solver using facial landmarks
- **Output**:
  - 3D position (X, Y, Z) in camera coordinate system
  - Euler angles (pitch, yaw, roll) in degrees
- **Derived Metrics**:
  - Movement variance (attention stability indicator)
  - Looking-away detection (|yaw| > 30° or |pitch| > 30°)
  - Posture consistency scoring

### Research & ML Applications

The multi-modal dataset enables:
- **Attention Lapse Detection**: Correlation analysis between head pose and missed targets
- **Fatigue Modeling**: Time-series analysis of blink rate trends
- **Error Prediction**: ML models using behavioral features to predict response errors
- **Posture Analysis**: Clustering analysis of head movement patterns
- **Multi-Modal Fusion**: Combining CV features with behavioral responses for richer insights

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

### Extensible Architecture (Tier 2/3 Features)

The modular pipeline design supports easy integration of additional ML models:

**Planned Computer Vision Features:**
- **Blink Duration Analysis**: Temporal blink length measurement (fatigue indicator)
- **PERCLOS (Percentage of Eye Closure)**: Drowsiness detection metric (% time eyes >80% closed)
- **Gaze Tracking**: Screen-space gaze estimation from eye landmarks
- **Pupillometry**: Pupil diameter tracking for cognitive load measurement
- **Facial Action Coding**: Emotion detection (frustration, boredom) via facial expression recognition
- **Advanced Eye Tracking**: Fixation stability, saccade detection, smooth pursuit analysis

**Data Engineering Enhancements:**
- **Real-time streaming**: Kafka/Redis integration for live data streaming
- **Database backend**: PostgreSQL/TimescaleDB for time-series optimization
- **Feature store**: Centralized feature repository for ML model training
- **Automated ML pipeline**: Model training and evaluation on longitudinal data

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

## Technical Implementation Details

**Performance Optimization:**
- High-precision timing using `time.perf_counter()` for microsecond-level accuracy
- Frame-locked rendering at 60 FPS with V-sync
- Non-blocking webcam capture with threaded I/O (prevents frame drops)
- Efficient CSV writing with buffered I/O

**Data Quality:**
- Timestamp synchronization between behavioral and tracking data
- Frame-level quality metrics (successful face detection rate)
- Missing data handling in aggregation pipeline

**Task Design:**
- No feedback provided during trials (prevents learning effects)
- First trial cannot be AX (requires prior context)
- Randomized trial sequences while maintaining target probability distribution
- Configurable timing parameters for different difficulty levels

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

