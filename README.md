# Event Sum Map Workflow

## Introduction

This workflow helps you to visualize where event activity is concentrated by building a grid heatmap from your EarthRanger events. Each grid cell shows either the **number of events** that fall inside it, or the **total of a numeric event field** you choose (for example, Number of Animals) — so you can see at a glance not just where events happen, but where their combined magnitude is highest.

**What this workflow does:**
- Downloads **events** from EarthRanger for your chosen time range and event types
- Resolves each event's details fields into the same titles you see in EarthRanger
- Overlays a grid on your events and, per cell, **counts events** or **sums a chosen numeric field** (the Aggregate Column)
- Classifies the per-cell totals into 10 color bands (green = low, red = high)
- Creates an interactive dashboard map with a legend, tooltips, and your chosen base layers
- Optionally splits the map into per-group views (by event type, month, region, and more)

**Who should use this:**
- Conservation managers monitoring where incidents or sightings concentrate
- Researchers analyzing the spatial distribution and intensity of event data
- Anyone needing a gridded "hotspot" map of event information stored in EarthRanger

## Prerequisites

Before using this workflow, you need:

1. **Ecoscope Desktop** installed on your computer
   - If you haven't installed it yet, please follow the installation instructions for Ecoscope Desktop

2. **EarthRanger Data Source** configured in Ecoscope Desktop
   - You must have already set up a connection to your EarthRanger server
   - Your data source should be configured with proper authentication credentials
   - You'll need to know the name of your configured data source (e.g., "mep_dev")

3. **Event Types with data** set up in EarthRanger
   - You need events recorded in your EarthRanger system for the period you want to analyze
   - If you plan to use the **Aggregate Column** option, the event type's report form must include a numeric field (for example, Number of Animals)
   - You can review your event types at `https://<your-site>.pamdas.org/admin/activity/eventtype/`

## Installation

1. Select "Workflow Templates" tab
2. Click "+ Add Template"
3. Copy and paste this URL https://github.com/wildlife-dynamics/event-sum-map and wait for the workflow template to be downloaded and initialized
4. The template will now appear in your available template list

## Configuration Guide

### Basic Configuration

#### 1. Workflow Details
Add information that will help to differentiate this workflow from another.

- **Workflow Name** (required): A descriptive name for this workflow run
  - Example: `Event Sum Map`
- **Workflow Description** (optional): Notes about what this run analyzes
  - Example: `Grid heatmap of per-cell event totals.`

#### 2. Data Source
Select the EarthRanger connection to pull events from.

- **Data Source** (required): One of your configured data sources
  - Example: `mep_dev`

#### 3. Time Range
Choose the period of time to analyze.

- **Since** (required): The start time
  - Example: `2026-07-01T00:00:00`
- **Until** (required): The end time
  - Example: `2026-07-31T23:59:59`
- **Timezone** (optional): The timezone used to interpret and display event times
  - Example: `Africa/Nairobi (UTC+03:00)` or `UTC (UTC+00:00)`

#### 4. Event Types
Specify the event type(s) to analyze.

- **Event Types** (optional): One or more event type values
  - Example: `wildlife_sighting_rep`
  - Note: Leave this section empty to analyze all event types
  - Note: If you are on Ecoscope Desktop, "Event Type" values can be found in your EarthRanger Admin site under Activity → Event Types

#### 5. Group Data
Configure how events are grouped and split into per-group dashboard views. Leave empty for a single combined map.

- **Category**: Split by a category value — **Event Type**, **Event Category**, or **Reported By**
- **Time**: Split by a time period — e.g. `%Y` (Year), `%B` (Month), `%Y-%m-%d` (Date)
- **Spatial**: Split by the regions of a spatial feature group configured in your EarthRanger site
  - Note: Each group value becomes its own map view in the dashboard, selectable from a dropdown

#### 6. Map Base Layers
Select tile layers to use as base layers in map outputs.

- **Base Maps** (optional): One or more tile layer URLs with an opacity each
  - Default: a topographic base layer with a semi-transparent satellite imagery layer on top
  - Note: The first layer in the list will be the bottom layer

#### 7. Event Sum Map
The gridded heatmap itself.

- **Aggregate Column** (optional): Event details field whose values are totaled per grid cell, using the field title shown in EarthRanger
  - Example: `Number of Animals`
  - Note: Leave empty to **count events** per cell instead
  - Note: The name must match the field title exactly, including capitalization, and the field must contain numeric values
  - Note: The map tooltip and legend take their label from this field — for example, `Number of Animals per Grid Cell` — or show `Total per Grid Cell` in count mode

### Advanced Configuration

These optional settings live in the **Advanced Configurations** sections and provide additional control:

#### Event Location Filter (Filter Data)
- **Bounding Box**: Only include events whose coordinates fall inside this bounding box
  - Default: the whole world (latitude -90 to 90, longitude -180 to 180)
- **Filter Exact Point Coordinates**: Exclude events recorded at these exact coordinates (e.g. known bad GPS fixes)

#### Density Grid Options (Event Sum Map)
- **Heatmap Layer Opacity**: Set heatmap layer transparency from 1 (fully visible) to 0 (hidden)
  - Default: `0.7`
- **Grid Cell Size**: Choose **Auto-scale** for an optimized grid cell size based on your data, or **Customize** to set a specific size
  - Default: `Auto-scale`
  - Custom default: `5000` (in the unit of the coordinate reference system below — meters for the default CRS)
  - Note: A smaller grid cell size provides more detail, while a larger size generalizes the data
- **Coordinate Reference System**: The CRS in which the grid is calculated
  - Default: `EPSG:3857`
  - Note: Must be a valid CRS authority code, for example `ESRI:53042`

## Running the Workflow

Once you've configured all the settings:

1. **Review your configuration**
   - Double-check your time range, data source, event types, and Aggregate Column spelling

2. **Save and run**
   - Click the "Submit" and the workflow will show up in "My Workflows" table button in Ecoscope Desktop
   - Click on "Run" and the workflow will begin processing

3. **Monitor progress and wait for completion**
   - You'll see status updates as the workflow runs
   - Processing time depends on:
     - The size of your date range
     - Number of event types and events in the system
     - Number of groups (each group renders its own map)
   - The workflow completes with status "Success" or "Failed"

## Understanding Your Results

After the workflow completes successfully, open the workflow dashboard.

### Visual Outputs (Dashboard)

The workflow creates an interactive dashboard with one main visualization:

#### Event Sum Map
- **Format**: Interactive grid heatmap over your chosen base layers
- **Features**:
  - Each grid cell is colored by its total, in 10 equal-interval bands from green (low) through yellow to red (high)
  - **Legend**: Shows the value range for each color band, titled `Total per Grid Cell` in count mode or `<Aggregate Column> per Grid Cell` in sum mode (for example, `Number of Animals per Grid Cell`)
  - **Interactive hover**: Shows the cell's exact total when you mouse over it, labeled `Total` or with your Aggregate Column name
  - **North arrow** (top-left) and zoom controls
- **Grouped views**: If you configured Group Data, use the dashboard's group selector to switch between per-group maps (e.g. one per event type, month, or region)

Note: Cells with no events are not drawn, so the heatmap only covers areas with activity.

## Common Use Cases & Examples

Here are some typical scenarios and how to configure the workflow for each:

### Example 1: Where are wildlife sightings concentrated?
**Goal**: Count wildlife sighting events per grid cell for a year.

**Configuration**:
- **Time Range**:
  - Since: `2015-01-01T00:00:00`
  - Until: `2015-12-31T23:59:59`
  - Timezone: `UTC (UTC+00:00)`
- **Event Types**: `wildlife_sighting_rep`
- **Aggregate Column**: leave empty (count mode)

**Result**:
- A single heatmap where each cell shows the number of sightings inside it
- Tooltip shows `Total`; legend reads `Total per Grid Cell`

---

### Example 2: How many animals were seen, and where?
**Goal**: Total the "Number of Animals" reported per grid cell, with a finer custom grid.

**Configuration**:
- **Time Range**:
  - Since: `2015-01-01T00:00:00`
  - Until: `2015-12-31T23:59:59`
  - Timezone: `UTC (UTC+00:00)`
- **Event Types**: `wildlife_sighting_rep`, `hwc_rep`
- **Aggregate Column**: `Number of Animals`
- **Advanced — Density Grid Options**:
  - Heatmap Layer Opacity: `0.5`
  - Grid Cell Size: `Customize`, `2500`

**Result**:
- Each cell shows the summed animal count rather than the number of events
- Tooltip and legend are labeled `Number of Animals` / `Number of Animals per Grid Cell`

---

### Example 3: Compare event types month by month
**Goal**: Separate heatmaps for each event type and each month.

**Configuration**:
- **Time Range**:
  - Since: `2015-01-01T00:00:00`
  - Until: `2015-12-31T23:59:59`
  - Timezone: `UTC (UTC+00:00)`
- **Event Types**: leave empty (all event types)
- **Group Data**: Category = `Event Type`, Time = `%B` (Month)

**Result**:
- The dashboard's group selector lets you flip between each event type × month combination
- Each view is its own count-mode heatmap

---

### Example 4: Total deforestation-alert confidence per cell
**Goal**: Sum the numeric "Confidence" detail of GFW Integrated Alert events for one month.

**Configuration**:
- **Time Range**:
  - Since: `2026-07-01T00:00:00`
  - Until: `2026-07-31T23:59:59`
  - Timezone: `Africa/Nairobi (UTC+03:00)`
- **Event Types**: `gfwgladalert`
- **Aggregate Column**: `Confidence`

**Result**:
- Cells with many high-confidence alerts stand out in red
- Tooltip and legend are labeled `Confidence` / `Confidence per Grid Cell`

## Troubleshooting

### Common Issues and Solutions

#### Workflow fails to start
**Problem**: The workflow fails immediately with an authentication or connection error.

**Solutions**:
- Verify your EarthRanger data source is configured correctly in Ecoscope Desktop
- Check that your username and password are still valid on the EarthRanger site
- Confirm you can reach your EarthRanger server in a web browser

#### No events returned / empty map
**Problem**: The workflow succeeds but the map is empty or missing.

**Solutions**:
- Widen your time range — there may be no events in the selected period
- Check the Event Types values match your EarthRanger site exactly (see Activity → Event Types in the EarthRanger admin)
- If you set a Bounding Box filter, confirm your events fall inside it
- Remember that only events with coordinates are mapped — events without a location are skipped

#### Aggregate Column not found or totals look wrong
**Problem**: The workflow fails at the totals step, or every cell shows an unexpected value.

**Solutions**:
- Enter the field's **title exactly as shown in EarthRanger** (for example `Number of Animals`, not `number_of_animals`), including capitalization
- Confirm the field actually exists on the event types you selected — it must be part of the report form
- The field must be numeric; text or choice fields cannot be totaled
- Leave the Aggregate Column empty if you just want to count events per cell

#### Grid looks too coarse or too fine
**Problem**: The heatmap cells are too large to show detail, or so small the map looks speckled.

**Solutions**:
- Switch Grid Cell Size from `Auto-scale` to `Customize` and try a different value (e.g. `2500` for more detail, `10000` for a broader picture)
- The size is in the unit of the Coordinate Reference System — meters for the default `EPSG:3857`

#### Workflow runs very slowly
**Problem**: The workflow takes a long time to complete.

**Solutions**:
- Note that the very first run after installation includes a one-time warm-up; later runs are faster
- Narrow your time range or select specific event types instead of all
- Reduce the number of groups — every group value renders its own map view

#### Spatial group views are missing
**Problem**: You added a Spatial grouper but no per-region views appear.

**Solutions**:
- Confirm the spatial feature group name matches the group configured in your EarthRanger site
- Verify the feature group contains region polygons and that your events fall inside them
- Check that your EarthRanger user has permission to read spatial features

## Author

Yun Wu

## License

BSD-3-Clause
