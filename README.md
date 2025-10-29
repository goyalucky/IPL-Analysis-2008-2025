# 🏏 IPL Analysis Dashboard (2008-2025)

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![IPL](https://img.shields.io/badge/IPL-2008--2025-blue?style=for-the-badge)](https://www.iplt20.com/)

> An interactive Power BI dashboard that brings IPL statistics to life with dynamic visualizations, season-wise filtering, and comprehensive player & team analytics.

![Dashboard Preview](./assets/dashboard-preview.png)

---

## 📋 Table of Contents
- [Overview](#-overview)
- [Features](#-features)
- [Business Objective](#-business-objective)
- [Key Performance Indicators](#-key-performance-indicators)
- [Dashboard Components](#-dashboard-components)
- [Technical Architecture](#️-technical-architecture)
- [Installation & Setup](#-installation--setup)
- [Usage Guide](#-usage-guide)
- [Data Sources](#-data-sources)
- [Screenshots](#-screenshots)
- [Technologies Used](#-technologies-used)
- [Future Enhancements](#-future-enhancements)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

---

## 🎯 Overview

The **IPL Analysis Dashboard** is a comprehensive business intelligence solution built with Power BI that analyzes 17+ years of Indian Premier League data (2008-2025). This interactive dashboard enables cricket analysts, team management, and enthusiasts to explore detailed performance metrics of teams and players across different IPL seasons.

### Why This Project?

- **Data-Driven Insights**: Transform raw IPL statistics into actionable insights
- **Interactive Analysis**: Season-by-season exploration with dynamic filtering
- **Visual Storytelling**: Beautiful visualizations with team logos and player images
- **Performance Tracking**: Monitor key performance indicators that matter most

---

## ✨ Features

### 🎨 Interactive Design
- **Dynamic Season Selector**: Instantly switch between IPL seasons (2008-2025)
- **Real-time Updates**: All KPIs and visuals update automatically based on season selection
- **Responsive Layouts**: Optimized viewing experience across different screen sizes

### 📊 Comprehensive Analytics
- Team performance tracking (Winners & Runners-up)
- Individual player achievements (Orange Cap, Purple Cap)
- Batting excellence metrics (Fours & Sixes leaders)
- Visual player cards with dynamic images
- Team logo integration for quick identification

### 🚀 Advanced Capabilities
- Cross-filtering and drill-through functionality
- Custom DAX measures for complex calculations
- Conditional formatting for better data interpretation
- Export capabilities for reports and presentations

---

## 🎯 Business Objective

To create an **interactive Power BI dashboard** that:

✅ Displays comprehensive insights into team and player performances across all IPL seasons  
✅ Enables users to select any IPL season using slicers/filters for instant, season-specific insights  
✅ Provides dynamic visualizations with team logos and player images  
✅ Delivers key performance metrics through intuitive KPI cards  
✅ Supports data-driven decision making for cricket analysts and enthusiasts  

---

## 📈 Key Performance Indicators

### Primary KPIs

#### 🏆 Winner Team
- **Displays**: Champion team name and logo
- **Updates**: Dynamically based on selected season
- **Visual**: Team badge with championship title

#### 🥈 Runner-Up Team
- **Displays**: Finalist team name and logo
- **Updates**: Dynamically based on selected season
- **Visual**: Team badge with runner-up designation

---

## 🎪 Dashboard Components

### 🟧 Orange Cap (Highest Run Scorer)

```
┌─────────────────────────────────┐
│  🟧 ORANGE CAP HOLDER           │
├─────────────────────────────────┤
│  Player Name                     │
│  Total Runs: XXXX               │
│  Team: [Team Name]              │
│  [Player Image]                 │
└─────────────────────────────────┘
```

**Metrics Displayed:**
- Player name
- Total runs scored in the season
- Team represented
- Player photograph

---

### 🟪 Purple Cap (Highest Wicket Taker)

```
┌─────────────────────────────────┐
│  🟪 PURPLE CAP HOLDER           │
├─────────────────────────────────┤
│  Player Name                     │
│  Total Wickets: XX              │
│  Team: [Team Name]              │
│  [Player Image]                 │
└─────────────────────────────────┘
```

**Metrics Displayed:**
- Player name
- Total wickets taken in the season
- Team represented
- Player photograph

---

### 🟩 Most Fours in Season

```
┌─────────────────────────────────┐
│  🟩 MOST FOURS                  │
├─────────────────────────────────┤
│  Player Name                     │
│  Total Fours: XXX               │
│  Team: [Team Name]              │
│  [Player Image]                 │
└─────────────────────────────────┘
```

**Metrics Displayed:**
- Player name
- Total fours hit
- Team represented
- Player photograph

---

### 🟦 Most Sixes in Season

```
┌─────────────────────────────────┐
│  🟦 MOST SIXES                  │
├─────────────────────────────────┤
│  Player Name                     │
│  Total Sixes: XXX               │
│  Team: [Team Name]              │
│  [Player Image]                 │
└─────────────────────────────────┘
```

**Metrics Displayed:**
- Player name
- Total sixes hit
- Team represented
- Player photograph

---

## 🛠️ Technical Architecture

### Data Model Structure

```
Fact Tables:
├── Match_Results
├── Batting_Stats
├── Bowling_Stats
└── Season_Summary

Dimension Tables:
├── Teams (TeamID, TeamName, Logo)
├── Players (PlayerID, PlayerName, Image)
└── Seasons (SeasonID, Year)

Relationships:
One-to-Many relationships connecting:
- Teams → Match_Results
- Players → Batting_Stats
- Players → Bowling_Stats
- Seasons → All Fact Tables
```

### Key DAX Measures

#### Winner Team
```DAX
Winner Team = 
CALCULATE(
    FIRSTNONBLANK(Match_Results[Winner], 1),
    Match_Results[Match_Type] = "Final",
    ALLEXCEPT(Match_Results, 'Seasons'[Year])
)
```

#### Orange Cap Holder
```DAX
Orange Cap Holder = 
VAR MaxRuns = 
    CALCULATE(
        MAX(Batting_Stats[Total_Runs]),
        ALLEXCEPT(Batting_Stats, 'Seasons'[Year])
    )
RETURN
    CALCULATE(
        FIRSTNONBLANK(Players[PlayerName], 1),
        Batting_Stats[Total_Runs] = MaxRuns
    )
```

#### Purple Cap Holder
```DAX
Purple Cap Holder = 
VAR MaxWickets = 
    CALCULATE(
        MAX(Bowling_Stats[Total_Wickets]),
        ALLEXCEPT(Bowling_Stats, 'Seasons'[Year])
    )
RETURN
    CALCULATE(
        FIRSTNONBLANK(Players[PlayerName], 1),
        Bowling_Stats[Total_Wickets] = MaxWickets
    )
```

#### Most Fours
```DAX
Most Fours Player = 
VAR MaxFours = 
    CALCULATE(
        MAX(Batting_Stats[Total_Fours]),
        ALLEXCEPT(Batting_Stats, 'Seasons'[Year])
    )
RETURN
    CALCULATE(
        FIRSTNONBLANK(Players[PlayerName], 1),
        Batting_Stats[Total_Fours] = MaxFours
    )
```

### Power BI Techniques Applied

- **Data Modeling**: Star schema with fact and dimension tables
- **DAX Functions**: CALCULATE, FILTER, ALLEXCEPT, TOPN, FIRSTNONBLANK
- **Relationships**: One-to-many with bi-directional cross-filtering where needed
- **Slicers**: Year-based filtering for season selection
- **Dynamic Images**: Image URLs stored in data model for player photos and team logos
- **KPI Cards**: Custom visual cards with conditional formatting
- **Bookmarks**: For navigation and switching between views
- **Tooltips**: Custom tooltips for detailed information on hover

---

## 💻 Installation & Setup

### Prerequisites

- **Power BI Desktop** (Latest Version)
  - Download: [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)
- **Minimum System Requirements**:
  - Windows 10 or later
  - 4 GB RAM (8 GB recommended)
  - 2 GB available disk space

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/yourusername/ipl-analysis-dashboard.git
   cd ipl-analysis-dashboard
   ```

2. **Open the Power BI File**
   - Navigate to the project folder
   - Double-click on `IPL_Analysis_2008-2025.pbix`
   - Power BI Desktop will launch automatically

3. **Refresh Data (if needed)**
   - Click on **Home** → **Refresh**
   - Wait for data to load completely

4. **Configure Data Sources (if prompted)**
   - Update file paths if data sources are stored locally
   - Enter credentials if connecting to online sources

---

## 📖 Usage Guide

### Getting Started

1. **Select a Season**
   - Use the **Year Slicer** at the top of the dashboard
   - Click on any year between 2008-2025
   - All visuals will update automatically

2. **Explore Team Performance**
   - View the **Winner** and **Runner-Up** teams with their logos
   - Hover over team badges for additional information

3. **Analyze Player Statistics**
   - Review **Orange Cap** holder (top run scorer)
   - Check **Purple Cap** holder (top wicket taker)
   - See leaders in **Fours** and **Sixes**

4. **Interactive Features**
   - Click on any visual to cross-filter other visuals
   - Use right-click for drill-through options
   - Export visuals or data as needed

### Navigation Tips

- **Reset Filters**: Click the eraser icon to clear all selections
- **Full Screen**: Press F11 or click the full-screen button
- **Print/Export**: File → Export → PDF or PowerPoint

---

## 📂 Data Sources

### Primary Data Sources

- **IPL Official Statistics**: Match results, player performances
- **Cricinfo/Cricbuzz**: Historical data and player information
- **Team Websites**: Official team logos and branding
- **Player Databases**: Images and biographical information

### Data Structure

```
/data
├── match_results.csv       # Match outcomes and finals data
├── batting_stats.csv       # Detailed batting statistics
├── bowling_stats.csv       # Detailed bowling statistics
├── teams.csv               # Team information and logos
├── players.csv             # Player information and images
└── seasons.csv             # Season metadata
```

### Data Update Frequency

- **Historical Data**: Complete (2008-2024)
- **2025 Season**: Updated as matches conclude
- **Refresh Schedule**: Weekly during IPL season

---

## 📸 Screenshots

### Dashboard Overview
![Full Dashboard](./assets/full-dashboard.png)
*Complete view of the IPL Analysis Dashboard*

### Season Filter in Action
![Season Filter](./assets/season-filter.png)
*Dynamic filtering by IPL season*

### KPI Cards Detail
![KPI Cards](./assets/kpi-cards.png)
*Detailed view of performance indicators*

### Player Statistics
![Player Stats](./assets/player-stats.png)
*Orange Cap and Purple Cap holders*

---

## 🔧 Technologies Used

| Technology | Purpose | Version |
|------------|---------|---------|
| ![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black) | Dashboard Development | Latest |
| ![DAX](https://img.shields.io/badge/DAX-217346?style=flat) | Data Calculations | - |
| ![Power Query](https://img.shields.io/badge/Power_Query-217346?style=flat) | Data Transformation | - |
| ![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoft-excel&logoColor=white) | Data Preparation | 2021+ |

### Key Power BI Features Used

- ✅ Data Modeling (Star Schema)
- ✅ DAX Measures and Calculated Columns
- ✅ Power Query (M Language)
- ✅ Custom Visuals
- ✅ Slicers and Filters
- ✅ Bookmarks and Navigation
- ✅ Dynamic Image Rendering
- ✅ Conditional Formatting
- ✅ Tooltips and Drill-through

---

## 🚀 Future Enhancements

### Planned Features

- [ ] **Player Comparison Tool**: Compare multiple players across seasons
- [ ] **Venue Analysis**: Performance metrics by stadium
- [ ] **Predictive Analytics**: ML-based match outcome predictions
- [ ] **Mobile App**: Power BI Mobile optimization
- [ ] **Real-time Updates**: Live match integration during IPL season
- [ ] **Historical Trends**: Multi-season trend analysis
- [ ] **Team vs Team**: Head-to-head comparison module
- [ ] **Fantasy League Integration**: Points calculator for fantasy cricket

### Version History

- **v1.0.0** (Current) - Initial release with core features
- **v1.1.0** (Planned) - Player comparison and venue analysis
- **v2.0.0** (Planned) - Predictive analytics and real-time updates

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### How to Contribute

1. **Fork the Project**
   ```bash
   git fork https://github.com/yourusername/ipl-analysis-dashboard.git
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/AmazingFeature
   ```

3. **Commit Your Changes**
   ```bash
   git commit -m 'Add some AmazingFeature'
   ```

4. **Push to the Branch**
   ```bash
   git push origin feature/AmazingFeature
   ```

5. **Open a Pull Request**

### Contribution Guidelines

- Follow existing code style and structure
- Update documentation for new features
- Test thoroughly before submitting
- Include screenshots for UI changes
- Write clear commit messages

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 [Your Name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

---

## 📞 Contact

**Project Maintainer**: [Your Name]

- 📧 Email: your.email@example.com
- 💼 LinkedIn: [linkedin.com/in/yourprofile](https://linkedin.com/in/yourprofile)
- 🐙 GitHub: [@yourusername](https://github.com/yourusername)
- 🌐 Portfolio: [yourwebsite.com](https://yourwebsite.com)

### Project Links

- **Repository**: [github.com/yourusername/ipl-analysis-dashboard](https://github.com/yourusername/ipl-analysis-dashboard)
- **Issues**: [github.com/yourusername/ipl-analysis-dashboard/issues](https://github.com/yourusername/ipl-analysis-dashboard/issues)
- **Discussions**: [github.com/yourusername/ipl-analysis-dashboard/discussions](https://github.com/yourusername/ipl-analysis-dashboard/discussions)

---

## 🙏 Acknowledgments

- **IPL** for providing comprehensive cricket statistics
- **Power BI Community** for tutorials and best practices
- **Cricket Enthusiasts** who provided feedback and suggestions
- **Open Source Contributors** for inspiring this project

---

## ⭐ Star History

If you find this project useful, please consider giving it a star! ⭐

[![Star History Chart](https://api.star-history.com/svg?repos=yourusername/ipl-analysis-dashboard&type=Date)](https://star-history.com/#yourusername/ipl-analysis-dashboard&Date)

---

<div align="center">

### Made with ❤️ for Cricket Analytics

**Happy Analyzing! 🏏📊**

[⬆ Back to Top](#-ipl-analysis-dashboard-2008-2025)

</div>
