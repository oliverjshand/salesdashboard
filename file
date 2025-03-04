<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sales Performance Dashboard</title>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
  <style>
    :root {
      --bg-primary: #0c0c14;
      --bg-secondary: #12121f;
      --bg-tertiary: #1a1a2e;
      --bg-card: #181824;
      --text-primary: #f8fafc;
      --text-secondary: #94a3b8;
      --accent-primary: #8b5cf6;
      --accent-secondary: #6366f1;
      --accent-success: #10b981;
      --accent-danger: #ef4444;
      --accent-warning: #f59e0b;
      --accent-info: #3b82f6;
      --card-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.2), 0 4px 6px -2px rgba(0, 0, 0, 0.1);
      --success: #22c55e;
      --error: #f87171;
      --border-color: #334155;
      --gradient-1: linear-gradient(135deg, #8b5cf6, #6366f1);
      --gradient-2: linear-gradient(135deg, #10b981, #3b82f6);
      --gradient-3: linear-gradient(135deg, #f59e0b, #ef4444);
      --gradient-4: linear-gradient(135deg, #3b82f6, #a855f7);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      background-color: var(--bg-primary);
      color: var(--text-primary);
      line-height: 1.5;
    }

    .dashboard-container {
      max-width: 1600px;
      margin: 0 auto;
      padding: 2rem;
    }

    h1, h2, h3, h4 {
      font-weight: 600;
      margin-bottom: 0.5rem;
    }

    .dashboard-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 2rem;
    }

    .dashboard-title {
      font-size: 2.25rem;
      font-weight: 800;
      background: var(--gradient-1);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    }

    .date-range {
      color: var(--text-secondary);
      font-size: 1.1rem;
    }

    /* Enhanced KPI Cards */
    .big-metrics-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1.5rem;
      margin-bottom: 2rem;
    }

    .big-metric-card {
      background-color: var(--bg-card);
      border-radius: 1rem;
      padding: 1.5rem;
      box-shadow: var(--card-shadow);
      position: relative;
      overflow: hidden;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }

    .big-metric-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 15px 30px -5px rgba(0, 0, 0, 0.3);
    }

    .big-metric-card::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 5px;
      background: var(--gradient-1);
    }

    .big-metric-card:nth-child(2)::before {
      background: var(--gradient-2);
    }

    .big-metric-label {
      color: var(--text-secondary);
      font-size: 1.25rem;
      margin-bottom: 0.5rem;
    }

    .big-metric-value {
      font-size: 3.5rem;
      font-weight: 800;
      margin-bottom: 0.75rem;
      align-self: center;
      text-align: center;
      width: 100%;
      background: linear-gradient(to right, #fff, #94a3b8);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      text-shadow: 0 2px 10px rgba(255, 255, 255, 0.1);
      transition: all 0.5s ease;
    }

    .big-metric-card:hover .big-metric-value {
      transform: scale(1.05);
      text-shadow: 0 2px 15px rgba(255, 255, 255, 0.2);
    }

    .big-metric-change {
      font-size: 1rem;
      display: flex;
      align-items: center;
      gap: 0.5rem;
      margin-top: auto;
      padding-top: 1rem;
      border-top: 1px solid var(--border-color);
      width: 100%;
    }

    /* Secondary Metrics Grid */
    .metrics-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 1rem;
      margin-bottom: 2rem;
    }

    .metric-card {
      background-color: var(--bg-card);
      border-radius: 0.75rem;
      padding: 1.25rem;
      box-shadow: var(--card-shadow);
      position: relative;
      overflow: hidden;
      transition: transform 0.3s ease;
    }

    .metric-card:hover {
      transform: translateY(-3px);
    }

    .metric-card::after {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 5px;
      height: 100%;
      background: var(--accent-info);
    }

    .metric-card:nth-child(1)::after { background: var(--accent-info); }
    .metric-card:nth-child(2)::after { background: var(--accent-success); }
    .metric-card:nth-child(3)::after { background: var(--accent-warning); }
    .metric-card:nth-child(4)::after { background: var(--accent-primary); }

    .metric-label {
      color: var(--text-secondary);
      font-size: 0.875rem;
      margin-bottom: 0.25rem;
    }

    .metric-value {
      font-size: 1.75rem;
      font-weight: 700;
      margin-bottom: 0.5rem;
    }

    .metric-change {
      font-size: 0.875rem;
      display: flex;
      align-items: center;
      gap: 0.25rem;
    }

    /* Charts and Tables */
    .grid-row {
      display: grid;
      grid-template-columns: 1fr;
      gap: 1.5rem;
      margin-bottom: 2rem;
    }

    .funnel-container {
      height: 300px;
      margin: 1rem 0;
    }

    .funnel-step {
      width: 100%;
      height: 60px;
      margin-bottom: 10px;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 20px;
      color: white;
      font-weight: 600;
      border-radius: 5px;
      overflow: hidden;
      transition: all 0.5s ease;
    }

    .funnel-step:hover {
      transform: translateX(5px);
    }

    .funnel-step-fill {
      position: absolute;
      top: 0;
      left: 0;
      height: 100%;
      background: var(--gradient-4);
      z-index: 1;
      transition: width 1s ease-in-out;
    }

    .funnel-step-content {
      position: relative;
      z-index: 2;
      display: flex;
      justify-content: space-between;
      width: 100%;
    }

    .card {
      background-color: var(--bg-card);
      border-radius: 0.75rem;
      padding: 1.5rem;
      box-shadow: var(--card-shadow);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      overflow: hidden;
      margin-bottom: 1.5rem;
    }

    .card:hover {
      transform: translateY(-3px);
      box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.2);
    }

    .card-header {
      border-bottom: 1px solid var(--border-color);
      padding-bottom: 1rem;
      margin-bottom: 1.5rem;
    }

    .section-title {
      font-size: 1.5rem;
      font-weight: 600;
      margin-bottom: 0.5rem;
      background: var(--gradient-1);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }

    .chart-card {
      min-height: 450px;
    }

    .chart-header {
      margin-bottom: 1rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid var(--border-color);
      padding-bottom: 0.75rem;
    }

    .chart-title {
      font-size: 1.25rem;
      font-weight: 600;
    }

    .chart-legend {
      display: flex;
      flex-wrap: wrap;
      gap: 1rem;
      margin-top: 0.5rem;
    }

    .legend-item {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      font-size: 0.875rem;
    }

    .legend-color {
      width: 12px;
      height: 12px;
      border-radius: 50%;
    }

    /* Team Performance Tables */
    .team-performance-card {
      min-height: 400px;
    }

    .team-performance-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;
      padding-bottom: 0.75rem;
      border-bottom: 1px solid var(--border-color);
    }

    .team-performance-title {
      font-size: 1.25rem;
      font-weight: 600;
    }

    .table-container {
      overflow-x: auto;
    }

    table {
      width: 100%;
      border-collapse: collapse;
    }

    th, td {
      padding: 0.75rem 1rem;
      text-align: left;
      border-bottom: 1px solid var(--border-color);
    }

    th {
      background-color: var(--bg-tertiary);
      color: var(--text-secondary);
      font-weight: 500;
      position: sticky;
      top: 0;
      z-index: 10;
    }

    tbody tr {
      transition: background-color 0.2s;
    }

    tbody tr:hover {
      background-color: var(--bg-tertiary);
    }

    /* Top performers bar chart */
    .top-performers {
      display: flex;
      align-items: flex-end;
      justify-content: space-between;
      height: 200px;
      margin-top: 1.5rem;
    }

    .performer-bar {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 100%;
    }

    .bar {
      width: 70%;
      margin-bottom: 0.5rem;
      background: var(--gradient-1);
      border-radius: 4px 4px 0 0;
      box-shadow: 0 5px 15px rgba(139, 92, 246, 0.3);
      transition: height 0.7s ease-in-out;
    }

    .performer-name {
      font-size: 0.75rem;
      text-align: center;
      width: 100%;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .performer-value {
      font-size: 0.875rem;
      font-weight: bold;
    }

    /* Prospects Table */
    .table-card {
      border-radius: 0.75rem;
      overflow: hidden;
    }

    .table-header {
      background-color: var(--bg-tertiary);
      padding: 1rem 1.5rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 1px solid var(--border-color);
    }

    .table-tools {
      display: flex;
      gap: 0.75rem;
      align-items: center;
    }

    /* Buttons, Inputs, and UI */
    .btn {
      background-color: var(--bg-tertiary);
      color: var(--text-primary);
      border: none;
      border-radius: 0.5rem;
      padding: 0.5rem 1rem;
      font-size: 0.875rem;
      cursor: pointer;
      transition: all 0.2s;
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }

    .btn:hover {
      background-color: var(--border-color);
    }

    .btn-primary {
      background-color: var(--accent-primary);
      color: white;
    }

    .btn-primary:hover {
      background-color: #7c3aed;
    }

    .btn-danger {
      background-color: var(--accent-danger);
      color: white;
    }

    .btn-danger:hover {
      background-color: #dc2626;
    }

    .search-input {
      background-color: var(--bg-tertiary);
      border: 1px solid var(--border-color);
      border-radius: 0.5rem;
      padding: 0.5rem 1rem;
      font-size: 0.875rem;
      color: var(--text-primary);
      min-width: 200px;
    }

    .search-input::placeholder {
      color: var(--text-secondary);
    }

    .search-input:focus {
      outline: none;
      border-color: var(--accent-primary);
      box-shadow: 0 0 0 2px rgba(139, 92, 246, 0.3);
    }

    /* Status Tags */
    .tag {
      display: inline-flex;
      align-items: center;
      border-radius: 1rem;
      padding: 0.25rem 0.75rem;
      font-size: 0.75rem;
      font-weight: 500;
    }

    .tag-contacted {
      background-color: rgba(59, 130, 246, 0.2);
      color: var(--accent-info);
    }

    .tag-scheduled {
      background-color: rgba(16, 185, 129, 0.2);
      color: var(--accent-success);
    }

    .tag-closed {
      background-color: rgba(34, 197, 94, 0.2);
      color: var(--success);
    }

    .tag-noshow {
      background-color: rgba(239, 68, 68, 0.2);
      color: var(--accent-danger);
    }

    .tag-pending {
      background-color: rgba(245, 158, 11, 0.2);
      color: var(--accent-warning);
    }

    /* Action Buttons */
    .action-btn {
      background: none;
      border: none;
      color: var(--text-secondary);
      cursor: pointer;
      transition: color 0.2s;
      padding: 0.25rem;
    }

    .action-btn:hover {
      color: var(--text-primary);
    }

    .edit-btn:hover {
      color: var(--accent-info);
    }

    .delete-btn:hover {
      color: var(--accent-danger);
    }

    /* View Filters */
    .view-filters {
      display: flex;
      gap: 0.5rem;
      margin-bottom: 1rem;
    }

    .view-btn {
      background: none;
      border: 1px solid var(--border-color);
      border-radius: 0.5rem;
      padding: 0.5rem 1rem;
      font-size: 0.875rem;
      color: var(--text-secondary);
      cursor: pointer;
      transition: all 0.2s;
    }

    .view-btn.active {
      background-color: var(--accent-primary);
      border-color: var(--accent-primary);
      color: white;
    }

    /* Filter Tags */
    .filter-tags {
      display: flex;
      flex-wrap: wrap;
      margin-bottom: 1rem;
    }

    .filter-tag {
      display: inline-flex;
      align-items: center;
      background-color: var(--bg-tertiary);
      border-radius: 0.5rem;
      padding: 0.25rem 0.75rem;
      font-size: 0.75rem;
      margin-right: 0.5rem;
      margin-bottom: 0.5rem;
    }

    .filter-tag button {
      background: none;
      border: none;
      color: var(--text-secondary);
      margin-left: 0.5rem;
      cursor: pointer;
      font-size: 0.875rem;
    }

    /* Modals */
    .modal {
      display: none;
      position: fixed;
      z-index: 100;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.7);
      opacity: 0;
      transition: opacity 0.3s ease;
    }

    .modal.show {
      opacity: 1;
    }

    .modal-content {
      background-color: var(--bg-secondary);
      border-radius: 0.75rem;
      max-width: 600px;
      width: 90%;
      margin: 10vh auto;
      padding: 2rem;
      box-shadow: 0 10px 15px rgba(0, 0, 0, 0.5);
      transform: translateY(-20px);
      transition: transform 0.3s ease;
    }

    .modal.show .modal-content {
      transform: translateY(0);
    }

    .modal-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 1.5rem;
    }

    .close-modal {
      background: none;
      border: none;
      color: var(--text-secondary);
      font-size: 1.5rem;
      cursor: pointer;
      transition: color 0.2s;
    }

    .close-modal:hover {
      color: var(--text-primary);
    }

    /* Forms */
    .form-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1rem;
      margin-bottom: 1rem;
    }

    .form-field {
      margin-bottom: 1rem;
    }

    .form-field label {
      display: block;
      margin-bottom: 0.5rem;
      font-size: 0.875rem;
      color: var(--text-secondary);
    }

    .form-input {
      width: 100%;
      background-color: var(--bg-tertiary);
      border: 1px solid var(--border-color);
      border-radius: 0.5rem;
      padding: 0.75rem 1rem;
      font-size: 1rem;
      color: var(--text-primary);
      transition: border-color 0.2s, box-shadow 0.2s;
    }

    .form-input:focus {
      outline: none;
      border-color: var(--accent-primary);
      box-shadow: 0 0 0 2px rgba(139, 92, 246, 0.3);
    }

    .form-input::placeholder {
      color: var(--text-secondary);
    }

    .form-input[type="date"] {
      appearance: none;
    }

    .form-select {
      position: relative;
    }

    .form-select::after {
      content: '▼';
      font-size: 0.8rem;
      color: var(--text-secondary);
      position: absolute;
      right: 1rem;
      top: 50%;
      transform: translateY(-50%);
      pointer-events: none;
    }

    .form-actions {
      display: flex;
      justify-content: flex-end;
      gap: 1rem;
      margin-top: 1.5rem;
    }

    /* Stats Formatting */
    .currency {
      font-family: monospace;
    }

    .sortable {
      cursor: pointer;
      user-select: none;
    }

    .sortable:hover {
      background-color: var(--border-color);
    }

    .sortable::after {
      content: "⇅";
      margin-left: 0.5rem;
      opacity: 0.5;
    }

    .sortable.asc::after {
      content: "↑";
      opacity: 1;
    }

    .sortable.desc::after {
      content: "↓";
      opacity: 1;
    }

    /* Stats and Indicators */
    .positive-change {
      color: var(--success);
    }

    .negative-change {
      color: var(--error);
    }

    /* Dropdown */
    .dropdown {
      position: relative;
    }

    .dropdown-content {
      display: none;
      position: absolute;
      right: 0;
      top: 100%;
      min-width: 160px;
      background-color: var(--bg-tertiary);
      border-radius: 0.5rem;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
      z-index: 1;
      margin-top: 0.5rem;
    }

    .dropdown-content a {
      color: var(--text-primary);
      padding: 0.75rem 1rem;
      text-decoration: none;
      display: block;
      transition: background-color 0.2s;
    }

    .dropdown-content a:hover {
      background-color: var(--border-color);
    }

    .show {
      display: block;
    }

    /* Empty state */
    .empty-state {
      padding: 3rem 1.5rem;
      text-align: center;
      color: var(--text-secondary);
    }

    .empty-state-icon {
      font-size: 3rem;
      margin-bottom: 1rem;
      opacity: 0.5;
    }

    /* Utilities */
    .pulse {
      animation: pulse 2s infinite;
    }

    @keyframes pulse {
      0% { opacity: 0.5; }
      50% { opacity: 1; }
      100% { opacity: 0.5; }
    }

    /* Tooltip */
    [data-tooltip] {
      position: relative;
      cursor: help;
    }

    [data-tooltip]:before,
    [data-tooltip]:after {
      position: absolute;
      visibility: hidden;
      opacity: 0;
      pointer-events: none;
      transition: all 0.3s ease;
      z-index: 99;
    }

    [data-tooltip]:before {
      content: "";
      border-width: 6px;
      border-style: solid;
      border-color: transparent transparent var(--bg-tertiary) transparent;
      left: 50%;
      bottom: 100%;
      margin-bottom: -12px;
      transform: translateX(-50%);
    }

    [data-tooltip]:after {
      content: attr(data-tooltip);
      background-color: var(--bg-tertiary);
      color: var(--text-primary);
      text-align: center;
      line-height: 1.3;
      padding: 8px 12px;
      min-width: 160px;
      border-radius: 4px;
      font-size: 0.875rem;
      font-weight: normal;
      left: 50%;
      bottom: 100%;
      margin-bottom: -6px;
      transform: translateX(-50%) translateY(-10px);
      box-shadow: 0 1px 15px rgba(0, 0, 0, 0.2);
    }

    [data-tooltip]:hover:before,
    [data-tooltip]:hover:after {
      visibility: visible;
      opacity: 1;
      transform: translateX(-50%) translateY(0);
    }

    /* Interactive Cards */
    .metrics-grid .metric-card {
      cursor: pointer;
    }

    .metrics-grid .metric-card:nth-child(1):hover {
      box-shadow: 0 0 15px rgba(59, 130, 246, 0.3);
    }

    .metrics-grid .metric-card:nth-child(2):hover {
      box-shadow: 0 0 15px rgba(16, 185, 129, 0.3);
    }

    .metrics-grid .metric-card:nth-child(3):hover {
      box-shadow: 0 0 15px rgba(245, 158, 11, 0.3);
    }

    .metrics-grid .metric-card:nth-child(4):hover {
      box-shadow: 0 0 15px rgba(139, 92, 246, 0.3);
    }

    /* Login related styles */
    #login-container {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.9);
      z-index: 9999;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    #logout-btn {
      position: fixed;
      top: 20px;
      right: 20px;
      z-index: 999;
      padding: 8px 16px;
      background: var(--accent-primary);
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-family: inherit;
      font-size: 14px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    /* Responsive adjustments */
    @media (max-width: 1400px) {
      .big-metrics-grid {
        grid-template-columns: 1fr;
      }
      
      .metrics-grid {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    @media (max-width: 768px) {
      .metrics-grid {
        grid-template-columns: 1fr;
      }
      
      .chart-grid, .form-grid {
        grid-template-columns: 1fr;
      }
      
      .dashboard-container {
        padding: 1rem;
      }
      
      .big-metric-value {
        font-size: 2.5rem;
      }
    }
  </style>
</head>
<body>
  <div class="dashboard-container">
    <div class="dashboard-header">
      <h1 class="dashboard-title">Sales Performance Dashboard</h1>
      <div class="date-range" id="date-range"></div>
    </div>
    
    <!-- Big Metrics Row -->
    <div class="big-metrics-grid">
      <div class="big-metric-card">
        <div class="big-metric-label">Total Cash Collected</div>
        <div class="big-metric-value" id="total-cash-collected">$0</div>
        <div class="big-metric-change" id="cash-change">
          <i class="fas fa-arrow-up"></i> 0% vs. last month
        </div>
      </div>
      
      <div class="big-metric-card">
        <div class="big-metric-label">Total Revenue Generated</div>
        <div class="big-metric-value" id="total-revenue-generated">$0</div>
        <div class="big-metric-change" id="revenue-change">
          <i class="fas fa-arrow-up"></i> 0% vs. last month
        </div>
      </div>
    </div>
    
    <!-- Secondary Metrics Row -->
    <div class="metrics-grid">
      <div class="metric-card" data-tooltip="Total unique leads in the database">
        <div class="metric-label">Total Leads</div>
        <div class="metric-value" id="total-leads">0</div>
        <div class="metric-change" id="conversion-rate">
          0% conversion rate
        </div>
      </div>
      
      <div class="metric-card" data-tooltip="Number of scheduled sales calls">
        <div class="metric-label">Sales Calls</div>
        <div class="metric-value" id="sales-calls">0</div>
        <div class="metric-change" id="show-rate">
          0% show rate
        </div>
      </div>
      
      <div class="metric-card" data-tooltip="Average value of closed deals">
        <div class="metric-label">Average Deal Size</div>
        <div class="metric-value" id="avg-deal-size">$0</div>
        <div class="metric-change" id="deal-size-change">
          0% vs. last month
        </div>
      </div>
      
      <div class="metric-card" data-tooltip="Based on 10% commission rate">
        <div class="metric-label">Total Commissions</div>
        <div class="metric-value" id="total-commissions">$0</div>
        <div class="metric-change" id="commission-rate">
          Based on 10% rate
        </div>
      </div>
    </div>
    
    <!-- Revenue Chart -->
    <div class="card chart-card">
      <div class="chart-header">
        <h3 class="chart-title">Revenue Trends</h3>
      </div>
      <canvas id="revenue-chart"></canvas>
    </div>
    
    <!-- Sales Funnel -->
    <div class="card">
      <div class="chart-header">
        <h3 class="chart-title">Sales Funnel</h3>
      </div>
      <div class="funnel-container" id="sales-funnel">
        <!-- Will be populated by JavaScript -->
      </div>
    </div>
    
    <!-- Setter Performance Section -->
    <div class="card-header">
      <h2 class="section-title">Setters Performance</h2>
    </div>
    
    <div class="card team-performance-card">
      <div class="team-performance-header">
        <h3 class="team-performance-title">Top Setters</h3>
      </div>
      <div class="top-performers" id="top-setters">
        <!-- Will be populated by JavaScript -->
      </div>
    </div>
    
    <div class="card">
      <div class="team-performance-header">
        <h3 class="team-performance-title">Setters Performance Metrics</h3>
      </div>
      <div class="table-container">
        <table id="setters-performance-table">
          <thead>
            <tr>
              <th>Setter Name</th>
              <th>Calls Set</th>
              <th>Show Rate</th>
              <th>Calls Showed</th>
              <th>No Shows</th>
              <th>Close Rate</th>
              <th>Avg. Deal Size</th>
              <th>Commission</th>
            </tr>
          </thead>
          <tbody id="setters-performance-body">
            <!-- Will be populated by JavaScript -->
          </tbody>
        </table>
      </div>
    </div>
    
    <!-- Closer Performance Section -->
    <div class="card-header">
      <h2 class="section-title">Closers Performance</h2>
    </div>
    
    <div class="card team-performance-card">
      <div class="team-performance-header">
        <h3 class="team-performance-title">Top Closers</h3>
      </div>
      <div class="top-performers" id="top-closers">
        <!-- Will be populated by JavaScript -->
      </div>
    </div>
    
    <div class="card">
      <div class="team-performance-header">
        <h3 class="team-performance-title">Closers Performance Metrics</h3>
      </div>
      <div class="table-container">
        <table id="closers-performance-table">
          <thead>
            <tr>
              <th>Closer Name</th>
              <th>Calls Taken</th>
              <th>Calls Closed</th>
              <th>Close Rate</th>
              <th>Cash Collected</th>
              <th>CC Per Call</th>
              <th>Avg. Deal Size</th>
              <th>Commission</th>
            </tr>
          </thead>
          <tbody id="closers-performance-body">
            <!-- Will be populated by JavaScript -->
          </tbody>
        </table>
      </div>
    </div>
    
    <!-- Prospects Database Section -->
    <div class="card-header">
      <h2 class="section-title">Prospects Database</h2>
    </div>
    
    <div class="view-filters">
      <button class="view-btn active" data-view="all">All Leads</button>
      <button class="view-btn" data-view="contacted">Contacted</button>
      <button class="view-btn" data-view="scheduled">Scheduled</button>
      <button class="view-btn" data-view="closed">Closed</button>
    </div>
    
    <div class="filter-tags" id="filter-tags"></div>
    
    <div class="card table-card">
      <div class="table-header">
        <h3>Prospect Database</h3>
        <div class="table-tools">
          <input type="text" class="search-input" id="search-input" placeholder="Search prospects...">
          <button class="btn btn-primary" id="add-lead-btn">
            <i class="fas fa-plus"></i> Add Lead
          </button>
          <div class="dropdown">
            <button class="btn" id="dropdown-btn">
              <i class="fas fa-ellipsis-v"></i>
            </button>
            <div class="dropdown-content" id="dropdown-menu">
              <a href="#" id="export-btn"><i class="fas fa-download" style="margin-right:.5rem"></i>Export CSV</a>
              <a href="#" id="clear-btn"><i class="fas fa-trash-alt" style="margin-right:.5rem"></i>Clear All Data</a>
            </div>
          </div>
        </div>
      </div>
      
      <div class="table-container">
        <table id="prospects-table">
          <thead>
            <tr>
              <th class="sortable" data-sort="name">Prospect Name</th>
              <th class="sortable" data-sort="date">Date In</th>
              <th class="sortable" data-sort="status">Call Outcome</th>
              <th class="sortable" data-sort="source">Lead Source</th>
              <th class="sortable" data-sort="setby">Set By</th>
              <th class="sortable" data-sort="closedby">Closed By</th>
              <th class="sortable" data-sort="cash">Cash Collected</th>
              <th class="sortable" data-sort="revenue">Revenue</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody id="prospects-body">
            <!-- Table content will be dynamically generated -->
          </tbody>
        </table>
        
        <div class="empty-state" id="empty-state" style="display:none">
          <div class="empty-state-icon">
            <i class="fas fa-clipboard pulse"></i>
          </div>
          <h3 style="margin-bottom:.5rem">No prospects yet</h3>
          <p style="color:var(--text-secondary);margin-bottom:1.5rem">Add your first prospect to get started</p>
          <button class="btn btn-primary" id="empty-add-btn">
            <i class="fas fa-plus"></i> Add Your First Prospect
          </button>
        </div>
      </div>
    </div>
  </div>
  
  <!-- Add/Edit Lead Modal -->
  <div class="modal" id="lead-modal">
    <div class="modal-content">
      <div class="modal-header">
        <h2 id="modal-title">Add New Lead</h2>
        <button class="close-modal" id="close-modal">&times;</button>
      </div>
      
      <form id="lead-form">
        <input type="hidden" id="lead-id">
        
        <div class="form-grid">
          <div class="form-field">
            <label for="prospect-name">Prospect Name</label>
            <input type="text" id="prospect-name" class="form-input" required>
          </div>
          
          <div class="form-field">
            <label for="date-in">Date In</label>
            <input type="date" id="date-in" class="form-input" required>
          </div>
        </div>
        
        <div class="form-grid">
          <div class="form-field form-select">
            <label for="call-outcome">Call Outcome</label>
            <select id="call-outcome" class="form-input">
              <option value="">Select outcome</option>
              <option value="contacted">Contacted</option>
              <option value="scheduled">Scheduled</option>
              <option value="closed">Closed Won</option>
              <option value="noshow">No Show</option>
              <option value="pending">Pending</option>
            </select>
          </div>
          
          <div class="form-field form-select">
            <label for="lead-source">Lead Source</label>
            <select id="lead-source" class="form-input">
              <option value="">Select source</option>
              <option value="website">Website</option>
              <option value="social">Social Media</option>
              <option value="referral">Referral</option>
              <option value="webinar">Webinar</option>
              <option value="paid">Paid Ads</option>
              <option value="other">Other</option>
            </select>
          </div>
        </div>
        
        <div class="form-grid">
          <div class="form-field">
            <label for="set-by">Set By</label>
            <input type="text" id="set-by" class="form-input" placeholder="Appointment setter name">
          </div>
          
          <div class="form-field">
            <label for="closed-by">Closed By</label>
            <input type="text" id="closed-by" class="form-input" placeholder="Sales closer name">
          </div>
        </div>
        
        <div class="form-grid">
          <div class="form-field">
            <label for="cash-collected">Cash Collected</label>
            <input type="number" id="cash-collected" class="form-input" min="0" step="0.01" placeholder="0.00">
          </div>
          
          <div class="form-field">
            <label for="revenue-generated">Revenue Generated</label>
            <input type="number" id="revenue-generated" class="form-input" min="0" step="0.01" placeholder="0.00">
          </div>
        </div>
        
        <div class="form-field">
          <label for="notes">Notes</label>
          <textarea id="notes" class="form-input" rows="3" placeholder="Add any additional notes here..."></textarea>
        </div>
        
        <div class="form-actions">
          <button type="button" class="btn" id="cancel-btn">Cancel</button>
          <button type="submit" class="btn btn-primary">Save Lead</button>
        </div>
      </form>
    </div>
  </div>
  
  <!-- Delete Confirmation Modal -->
  <div class="modal" id="delete-modal">
    <div class="modal-content" style="max-width:400px">
      <div class="modal-header">
        <h2>Confirm Deletion</h2>
        <button class="close-modal" id="close-delete-modal">&times;</button>
      </div>
      
      <p style="margin-bottom:1.5rem">Are you sure you want to delete this prospect? This action cannot be undone.</p>
      
      <div class="form-actions">
        <button class="btn" id="cancel-delete-btn">Cancel</button>
        <button class="btn btn-danger" id="confirm-delete-btn">Delete</button>
      </div>
    </div>
  </div>
  
  <script>
    // Firebase Configuration
    const firebaseConfig = {
      apiKey: "AIzaSyDfJuCCCB7n_EMp7SWaViRmrnUWivVLqfY",
      authDomain: "sales-dashboard-backend-85a76.firebaseapp.com",
      projectId: "sales-dashboard-backend-85a76",
      storageBucket: "sales-dashboard-backend-85a76.firebasestorage.app",
      messagingSenderId: "799110733024",
      appId: "1:799110733024:web:47e69564cc31024bae8485"
    };
    
    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
    
    // Authentication functions
    function createLoginUI() {
      console.log("Creating login UI");
      const loginHTML = `
        <div id="login-container">
          <div style="background: #12121f; padding: 2rem; border-radius: 0.75rem; width: 90%; max-width: 400px;">
            <h2 style="color: #f8fafc; margin-bottom: 1.5rem;">Sales Dashboard Login</h2>
            <input type="password" id="password-input" placeholder="Enter password" style="width: 100%; padding: 0.75rem; margin-bottom: 1rem; border-radius: 0.5rem; border: 1px solid #334155; background: #1a1a2e; color: #f8fafc;">
            <button id="login-button" style="width: 100%; padding: 0.75rem; background: #8b5cf6; color: white; border: none; border-radius: 0.5rem; cursor: pointer;">Login</button>
            <p id="login-error" style="color: #ef4444; margin-top: 1rem; display: none;">Incorrect password</p>
          </div>
        </div>
      `;
      
      const loginDiv = document.createElement('div');
      loginDiv.innerHTML = loginHTML;
      document.body.appendChild(loginDiv);
      
      document.getElementById('login-button').addEventListener('click', attemptLogin);
      document.getElementById('password-input').addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
          attemptLogin();
        }
      });
    }
    
    function attemptLogin() {
      const password = document.getElementById('password-input').value.trim();
      console.log('Attempting login with password');
      
      if (password === 'admin12345') {
        console.log('Password correct');
        localStorage.setItem('dashboard_authenticated', 'true');
        
        // Remove login overlay
        const loginContainer = document.getElementById('login-container');
        if (loginContainer) {
          loginContainer.remove();
        }
        
        // Add logout button
        addLogoutButton();
      } else {
        console.log('Password incorrect');
        document.getElementById('login-error').style.display = 'block';
      }
    }
    
    function addLogoutButton() {
      // Check if logout button already exists
      if (document.getElementById('logout-btn')) return;
      
      // Create the logout button
      const logoutBtn = document.createElement('button');
      logoutBtn.id = 'logout-btn';
      logoutBtn.innerHTML = '<i class="fas fa-sign-out-alt"></i> Logout';
      
      logoutBtn.onclick = function() {
        localStorage.removeItem('dashboard_authenticated');
        console.log('Logged out successfully');
        window.location.reload(); // Simple reload on logout
      };
      
      // Add to document body
      document.body.appendChild(logoutBtn);
      console.log("Logout button added");
    }
    
    // Dashboard script with enhanced KPI tracking
    document.addEventListener('DOMContentLoaded', function() {
      let prospects = [];
      let currentView = 'all';
      let currentSort = { field: 'date', direction: 'desc' };
      let searchTerm = '';
      let currentEditId = null;
      let revenueChart;
      
      // Update date range display
      updateDateRange();
      
      // First check authentication
      if (localStorage.getItem('dashboard_authenticated') !== 'true') {
        console.log("User not authenticated");
        createLoginUI();
      } else {
        console.log("User already authenticated");
        addLogoutButton();
      }
      
      // Load data and initialize dashboard
      loadData();
      initCharts();
      setupEventListeners();
      checkForZapierData();
      
      // Update date range display
      function updateDateRange() {
        const now = new Date();
        const monthNames = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
        const currentMonth = monthNames[now.getMonth()];
        const currentYear = now.getFullYear();
        
        document.getElementById('date-range').textContent = `${currentMonth} ${currentYear}`;
      }
      
      // Load data from Firebase
      function loadData() {
        console.log("Loading data from Firebase");
        try {
          db.collection('prospects').get()
            .then(snapshot => {
              prospects = [];
              snapshot.forEach(doc => {
                prospects.push({
                  id: doc.id,
                  ...doc.data()
                });
              });
              console.log(`Loaded ${prospects.length} prospects from Firebase`);
              
              renderTable();
              updateAllMetrics();
              updateCharts();
              updateSettersPerformance();
              updateClosersPerformance();
              updateFunnel();
            })
            .catch(error => {
              console.error("Error loading from Firebase:", error);
              // Try to load from localStorage as fallback
              loadFromLocalStorageFallback();
            });
        } catch (error) {
          console.error("Unexpected error in loadDataFromFirebase:", error);
          loadFromLocalStorageFallback();
        }
      }
      
      function loadFromLocalStorageFallback() {
        console.log("Attempting to load from localStorage as fallback");
        try {
          const savedData = localStorage.getItem('mentorshipProspects');
          if (savedData) {
            prospects = JSON.parse(savedData);
            console.log(`Loaded ${prospects.length} prospects from localStorage`);
            
            renderTable();
            updateAllMetrics();
            updateCharts();
            updateSettersPerformance();
            updateClosersPerformance();
            updateFunnel();
          }
        } catch (fallbackError) {
          console.error("Error loading from localStorage:", fallbackError);
        }
      }
      
      // Save data to Firebase
      function saveData() {
        console.log("Saving data to Firebase");
        try {
          // Create a batch to handle multiple operations
          const batch = db.batch();
          
          // First get existing documents to delete them
          db.collection('prospects').get()
            .then(snapshot => {
              // Delete existing documents
              snapshot.forEach(doc => {
                batch.delete(doc.ref);
              });
              
              // Then add all current prospects
              prospects.forEach(prospect => {
                const docRef = db.collection('prospects').doc(prospect.id || generateId());
                batch.set(docRef, { ...prospect });
              });
              
              // Commit the batch
              return batch.commit();
            })
            .then(() => {
              console.log(`Saved ${prospects.length} prospects to Firebase`);
            })
            .catch(error => {
              console.error("Error saving to Firebase:", error);
              saveToLocalStorageFallback();
            });
        } catch (error) {
          console.error("Unexpected error in saveDataToFirebase:", error);
          saveToLocalStorageFallback();
        }
      }
      
      function saveToLocalStorageFallback() {
        // Save to localStorage as fallback
        try {
          localStorage.setItem('mentorshipProspects', JSON.stringify(prospects));
          console.log("Saved data to localStorage as fallback");
        } catch (fallbackError) {
          console.error("Error saving to localStorage:", fallbackError);
          alert("There was an issue saving your data. Please export a backup to avoid data loss.");
        }
      }
      
      function generateId() {
        return Date.now().toString(36) + Math.random().toString(36).substr(2);
      }
      
      // Zapier data handler that can parse messy URL parameters
      function checkForZapierData() {
        try {
          console.log("Checking for Zapier data in URL...");
          const urlParams = new URLSearchParams(window.location.search);
          
          // Log all URL parameters for debugging
          console.log("URL parameters found:", Object.fromEntries(urlParams));
          
          // Get parameters and clean them up
          let prospectName = urlParams.get('prospect_name') || '';
          let dateIn = urlParams.get('date_in') || '';
          
          // Clean up prospect name (extract from "1. Fullname:Bakefrog" format)
          if (prospectName.includes(':')) {
            prospectName = prospectName.split(':').pop().trim();
          }
          
          // Clean up date (convert from "March 17, 2025" to YYYY-MM-DD)
          if (dateIn) {
            // Handle various date formats
            try {
              if (dateIn.includes(':')) {
                // Format: "1. Booking Date:March 17, 2025"
                dateIn = dateIn.split(':').pop().split(',')[0].trim();
              }
              
              // Parse the date string
              const parsedDate = new Date(dateIn);
              if (!isNaN(parsedDate.getTime())) {
                // Format as YYYY-MM-DD
                const year = parsedDate.getFullYear();
                const month = String(parsedDate.getMonth() + 1).padStart(2, '0');
                const day = String(parsedDate.getDate()).padStart(2, '0');
                dateIn = `${year}-${month}-${day}`;
              } else {
                // If parsing fails, use today's date
                const today = new Date();
                const year = today.getFullYear();
                const month = String(today.getMonth() + 1).padStart(2, '0');
                const day = String(today.getDate()).padStart(2, '0');
                dateIn = `${year}-${month}-${day}`;
              }
            } catch (dateError) {
              console.error("Error parsing date:", dateError);
              // Use current date as fallback
              const today = new Date();
              dateIn = today.toISOString().split('T')[0];
            }
          } else {
            // Use current date if no date provided
            dateIn = new Date().toISOString().split('T')[0];
          }
          
          // Clean up other parameters
          let status = urlParams.get('status') || 'pending';
          if (status.includes(':')) {
            status = status.split(':').pop().trim();
          }
          
          let source = urlParams.get('source') || 'Website';
          if (source.includes(':')) {
            source = source.split(':').pop().trim();
          }
          
          // Get remaining parameters with fallbacks
          const cashValue = urlParams.get('cash') || '0';
          const cash = parseFloat(cashValue.includes(':') ? cashValue.split(':').pop().trim() : cashValue) || 0;
          
          const revenueValue = urlParams.get('revenue') || '0';
          const revenue = parseFloat(revenueValue.includes(':') ? revenueValue.split(':').pop().trim() : revenueValue) || 0;
          
          let setby = urlParams.get('setby') || 'Zapier';
          if (setby.includes(':')) {
            setby = setby.split(':').pop().trim();
          }
          
          let closedby = urlParams.get('closedby') || '';
          if (closedby.includes(':')) {
            closedby = closedby.split(':').pop().trim();
          }
          
          let notes = urlParams.get('notes') || 'Added automatically via Zapier';
          if (notes.includes(':')) {
            notes = notes.split(':').pop().trim();
          }
          
          if (prospectName) {
            // Create the lead with all available data
            const newLead = {
              name: prospectName,
              date: dateIn,
              status: status,
              source: source,
              cash: cash,
              revenue: revenue,
              setby: setby,
              closedby: closedby,
              notes: notes
            };
            
            console.log("Adding new lead from Zapier:", newLead);
            addLead(newLead);
            
            // Clear URL parameters
            window.history.replaceState({}, document.title, window.location.pathname);
            
            // Show confirmation
            alert("New lead added from Zapier: " + prospectName);
            return true;
          } else {
            console.log("No valid Zapier data found in URL");
            return false;
          }
        } catch (error) {
          console.error("Error processing Zapier data:", error);
          return false;
        }
      }
      
      function addLead(lead) {
        prospects.push({
          id: generateId(),
          name: lead.name,
          date: lead.date,
          status: lead.status || '',
          source: lead.source || '',
          cash: parseFloat(lead.cash) || 0,
          revenue: parseFloat(lead.revenue) || 0,
          setby: lead.setby || '',
          closedby: lead.closedby || '',
          notes: lead.notes || ''
        });
        saveData();
        renderTable();
        updateAllMetrics();
        updateCharts();
        updateSettersPerformance();
        updateClosersPerformance();
        updateFunnel();
      }
      
      function updateLead(id, updatedLead) {
        const index = prospects.findIndex(p => p.id === id);
        if (index !== -1) {
          prospects[index] = {
            ...prospects[index],
            name: updatedLead.name,
            date: updatedLead.date,
            status: updatedLead.status,
            source: updatedLead.source,
            cash: parseFloat(updatedLead.cash) || 0,
            revenue: parseFloat(updatedLead.revenue) || 0,
            setby: updatedLead.setby,
            closedby: updatedLead.closedby,
            notes: updatedLead.notes
          };
          saveData();
          renderTable();
          updateAllMetrics();
          updateCharts();
          updateSettersPerformance();
          updateClosersPerformance();
          updateFunnel();
        }
      }
      
      function deleteLead(id) {
        prospects = prospects.filter(p => p.id !== id);
        saveData();
        renderTable();
        updateAllMetrics();
        updateCharts();
        updateSettersPerformance();
        updateClosersPerformance();
        updateFunnel();
      }
      
      function renderTable() {
        const tableBody = document.getElementById('prospects-body');
        const emptyState = document.getElementById('empty-state');
        tableBody.innerHTML = '';
        const filteredProspects = filterProspects();
        const sortedProspects = sortProspects(filteredProspects);
        
        if (sortedProspects.length === 0) {
          tableBody.style.display = 'none';
          emptyState.style.display = 'block';
          return;
        }
        
        tableBody.style.display = '';
        emptyState.style.display = 'none';
        
        sortedProspects.forEach(prospect => {
          const row = document.createElement('tr');
          
          const nameCell = document.createElement('td');
          nameCell.textContent = prospect.name || '';
          row.appendChild(nameCell);
          
          const dateCell = document.createElement('td');
          const formattedDate = prospect.date ? new Date(prospect.date).toLocaleDateString() : '';
          dateCell.textContent = formattedDate;
          row.appendChild(dateCell);
          
          const statusCell = document.createElement('td');
          if (prospect.status) {
            const statusMap = {
              'contacted': { class: 'tag-contacted', label: 'Contacted' },
              'scheduled': { class: 'tag-scheduled', label: 'Scheduled' },
              'closed': { class: 'tag-closed', label: 'Closed Won' },
              'noshow': { class: 'tag-noshow', label: 'No Show' },
              'pending': { class: 'tag-pending', label: 'Pending' }
            };
            const status = statusMap[prospect.status] || { class: '', label: prospect.status };
            const statusTag = document.createElement('span');
            statusTag.className = `tag ${status.class}`;
            statusTag.textContent = status.label;
            statusCell.appendChild(statusTag);
          } else {
            statusCell.textContent = '—';
            statusCell.style.color = 'var(--text-secondary)';
          }
          row.appendChild(statusCell);
          
          const sourceCell = document.createElement('td');
          sourceCell.textContent = prospect.source || '—';
          if (!prospect.source) sourceCell.style.color = 'var(--text-secondary)';
          row.appendChild(sourceCell);
          
          const setByCell = document.createElement('td');
          setByCell.textContent = prospect.setby || '—';
          if (!prospect.setby) setByCell.style.color = 'var(--text-secondary)';
          row.appendChild(setByCell);
          
          const closedByCell = document.createElement('td');
          closedByCell.textContent = prospect.closedby || '—';
          if (!prospect.closedby) closedByCell.style.color = 'var(--text-secondary)';
          row.appendChild(closedByCell);
          
          const cashCell = document.createElement('td');
          cashCell.className = 'currency';
          cashCell.textContent = formatCurrency(prospect.cash);
          row.appendChild(cashCell);
          
          const revenueCell = document.createElement('td');
          revenueCell.className = 'currency';
          revenueCell.textContent = formatCurrency(prospect.revenue);
          row.appendChild(revenueCell);
          
          const actionsCell = document.createElement('td');
          actionsCell.style.display = 'flex';
          actionsCell.style.gap = '0.5rem';
          
          const editBtn = document.createElement('button');
          editBtn.className = 'action-btn edit-btn';
          editBtn.innerHTML = '<i class="fas fa-edit"></i>';
          editBtn.onclick = function() {
            openEditModal(prospect.id);
          };
          actionsCell.appendChild(editBtn);
          
          const deleteBtn = document.createElement('button');
          deleteBtn.className = 'action-btn delete-btn';
          deleteBtn.innerHTML = '<i class="fas fa-trash-alt"></i>';
          deleteBtn.onclick = function() {
            openDeleteModal(prospect.id);
          };
          actionsCell.appendChild(deleteBtn);
          
          row.appendChild(actionsCell);
          tableBody.appendChild(row);
        });
        
        updateFilterTags();
      }
      
      function filterProspects() {
        return prospects.filter(prospect => {
          if (currentView !== 'all' && prospect.status !== currentView) {
            return false;
          }
          if (searchTerm) {
            const searchLower = searchTerm.toLowerCase();
            const nameMatch = prospect.name && prospect.name.toLowerCase().includes(searchLower);
            const setByMatch = prospect.setby && prospect.setby.toLowerCase().includes(searchLower);
            const closedByMatch = prospect.closedby && prospect.closedby.toLowerCase().includes(searchLower);
            const notesMatch = prospect.notes && prospect.notes.toLowerCase().includes(searchLower);
            return nameMatch || setByMatch || closedByMatch || notesMatch;
          }
          return true;
        });
      }
      
      function sortProspects(prospectsToSort) {
        return [...prospectsToSort].sort((a, b) => {
          let aValue = a[currentSort.field];
          let bValue = b[currentSort.field];
          
          if (currentSort.field === 'date') {
            aValue = new Date(aValue || '1970-01-01').getTime();
            bValue = new Date(bValue || '1970-01-01').getTime();
          } else if (currentSort.field === 'cash' || currentSort.field === 'revenue') {
            aValue = parseFloat(aValue) || 0;
            bValue = parseFloat(bValue) || 0;
          } else {
            aValue = String(aValue || '').toLowerCase();
            bValue = String(bValue || '').toLowerCase();
          }
          
          return currentSort.direction === 'asc' ? (aValue > bValue ? 1 : -1) : (aValue < bValue ? 1 : -1);
        });
      }
      
      function updateFilterTags() {
        const container = document.getElementById('filter-tags');
        container.innerHTML = '';
        
        if (currentView !== 'all') {
          const viewLabels = {
            'contacted': 'Contacted',
            'scheduled': 'Scheduled',
            'closed': 'Closed Won',
            'noshow': 'No Show',
            'pending': 'Pending'
          };
          
          const tag = document.createElement('div');
          tag.className = 'filter-tag';
          tag.innerHTML = `Status: ${viewLabels[currentView] || currentView}`;
          
          const clearBtn = document.createElement('button');
          clearBtn.textContent = '×';
          clearBtn.onclick = clearViewFilter;
          tag.appendChild(clearBtn);
          
          container.appendChild(tag);
        }
        
        if (searchTerm) {
          const tag = document.createElement('div');
          tag.className = 'filter-tag';
          tag.innerHTML = `Search: "${searchTerm}"`;
          
          const clearBtn = document.createElement('button');
          clearBtn.textContent = '×';
          clearBtn.onclick = clearSearchFilter;
          tag.appendChild(clearBtn);
          
          container.appendChild(tag);
        }
      }
      
      function clearViewFilter() {
        currentView = 'all';
        document.querySelectorAll('.view-btn').forEach(btn => {
          btn.classList.toggle('active', btn.dataset.view === 'all');
        });
        renderTable();
      }
      
      function clearSearchFilter() {
        searchTerm = '';
        document.getElementById('search-input').value = '';
        renderTable();
      }
      
      // Unified function to update all metrics
      function updateAllMetrics() {
        // Get current and previous month data
        const now = new Date();
        const currentMonth = now.getMonth();
        const currentYear = now.getFullYear();
        
        const currentMonthProspects = prospects.filter(p => {
          if (!p.date) return false;
          const date = new Date(p.date);
          return date.getMonth() === currentMonth && date.getFullYear() === currentYear;
        });
        
        const prevMonth = currentMonth === 0 ? 11 : currentMonth - 1;
        const prevYear = currentMonth === 0 ? currentYear - 1 : currentYear;
        
        const prevMonthProspects = prospects.filter(p => {
          if (!p.date) return false;
          const date = new Date(p.date);
          return date.getMonth() === prevMonth && date.getFullYear() === prevYear;
        });
        
        // Calculate basic metrics
        const totalCash = prospects.reduce((sum, p) => sum + (parseFloat(p.cash) || 0), 0);
        const totalRevenue = prospects.reduce((sum, p) => sum + (parseFloat(p.revenue) || 0), 0);
        const cashMtd = currentMonthProspects.reduce((sum, p) => sum + (parseFloat(p.cash) || 0), 0);
        const revenueMtd = currentMonthProspects.reduce((sum, p) => sum + (parseFloat(p.revenue) || 0), 0);
        const cashPrev = prevMonthProspects.reduce((sum, p) => sum + (parseFloat(p.cash) || 0), 0);
        const revenuePrev = prevMonthProspects.reduce((sum, p) => sum + (parseFloat(p.revenue) || 0), 0);
        
        // Calculate percentage changes
        const cashChange = cashPrev ? ((cashMtd - cashPrev) / cashPrev * 100) : 0;
        const revenueChange = revenuePrev ? ((revenueMtd - revenuePrev) / revenuePrev * 100) : 0;
        
        // Calculate lead metrics
        const totalLeads = prospects.length;
        const closedLeads = prospects.filter(p => p.status === 'closed').length;
        const conversionRate = totalLeads ? (closedLeads / totalLeads * 100) : 0;
        
        // Calculate call metrics
        const scheduledCalls = prospects.filter(p => p.status === 'scheduled' || p.status === 'closed' || p.status === 'noshow').length;
        const showedCalls = prospects.filter(p => p.status === 'closed' || p.status === 'contacted').length;
        const showRate = scheduledCalls ? (showedCalls / scheduledCalls * 100) : 0;
        
        // Calculate deal metrics
        const avgDealSize = closedLeads ? (totalCash / closedLeads) : 0;
        const prevClosedLeads = prevMonthProspects.filter(p => p.status === 'closed').length;
        const prevAvgDealSize = prevClosedLeads ? (cashPrev / prevClosedLeads) : 0;
        const dealSizeChange = prevAvgDealSize ? ((avgDealSize - prevAvgDealSize) / prevAvgDealSize * 100) : 0;
        
        // Calculate commission (10% of cash collected)
        const commissionRate = 0.10; // 10%
        const totalCommission = totalCash * commissionRate;
        
        // Update the UI with all metrics
        document.getElementById('total-cash-collected').textContent = formatCurrency(totalCash);
        document.getElementById('total-revenue-generated').textContent = formatCurrency(totalRevenue);
        document.getElementById('total-leads').textContent = totalLeads;
        document.getElementById('sales-calls').textContent = scheduledCalls;
        document.getElementById('avg-deal-size').textContent = formatCurrency(avgDealSize);
        document.getElementById('total-commissions').textContent = formatCurrency(totalCommission);
        
        // Update change indicators
        document.getElementById('cash-change').innerHTML = getCashChangeHTML(cashChange);
        document.getElementById('revenue-change').innerHTML = getRevenueChangeHTML(revenueChange);
        document.getElementById('conversion-rate').textContent = `${conversionRate.toFixed(1)}% conversion rate`;
        document.getElementById('show-rate').textContent = `${showRate.toFixed(1)}% show rate`;
        document.getElementById('deal-size-change').innerHTML = getDealSizeChangeHTML(dealSizeChange);
        document.getElementById('commission-rate').textContent = `Based on ${(commissionRate * 100).toFixed(0)}% rate`;
      }
      
      function getCashChangeHTML(change) {
        if (change > 0) {
          return `<i class="fas fa-arrow-up"></i> <span class="positive-change">${change.toFixed(1)}%</span> vs. last month`;
        } else if (change < 0) {
          return `<i class="fas fa-arrow-down"></i> <span class="negative-change">${Math.abs(change).toFixed(1)}%</span> vs. last month`;
        } else {
          return `<i class="fas fa-minus"></i> 0% vs. last month`;
        }
      }
      
      function getRevenueChangeHTML(change) {
        if (change > 0) {
          return `<i class="fas fa-arrow-up"></i> <span class="positive-change">${change.toFixed(1)}%</span> vs. last month`;
        } else if (change < 0) {
          return `<i class="fas fa-arrow-down"></i> <span class="negative-change">${Math.abs(change).toFixed(1)}%</span> vs. last month`;
        } else {
          return `<i class="fas fa-minus"></i> 0% vs. last month`;
        }
      }
      
      function getDealSizeChangeHTML(change) {
        if (change > 0) {
          return `<i class="fas fa-arrow-up"></i> <span class="positive-change">${change.toFixed(1)}%</span> vs. last month`;
        } else if (change < 0) {
          return `<i class="fas fa-arrow-down"></i> <span class="negative-change">${Math.abs(change).toFixed(1)}%</span> vs. last month`;
        } else {
          return `<i class="fas fa-minus"></i> 0% vs. last month`;
        }
      }
      
      function initCharts() {
        try {
          Chart.defaults.color = 'rgba(248, 250, 252, 0.8)';
          Chart.defaults.borderColor = 'rgba(51, 65, 85, 0.7)';
          
          // Revenue chart
          const revenueCtx = document.getElementById('revenue-chart').getContext('2d');
          revenueChart = new Chart(revenueCtx, {
            type: 'line',
            data: {
              labels: [],
              datasets: [{
                label: 'Cash Collected',
                borderColor: '#8b5cf6',
                backgroundColor: 'rgba(139, 92, 246, 0.2)',
                data: [],
                tension: 0.4,
                fill: true,
                borderWidth: 3
              }, {
                label: 'Revenue Generated',
                borderColor: '#10b981',
                backgroundColor: 'rgba(16, 185, 129, 0.2)',
                data: [],
                tension: 0.4,
                fill: true,
                borderWidth: 3
              }]
            },
            options: {
              responsive: true,
              maintainAspectRatio: false,
              animation: {
                duration: 750 // Limit animation duration to fix expanding issue
              },
              plugins: {
                legend: {
                  position: 'top',
                },
                tooltip: {
                  mode: 'index',
                  intersect: false,
                  backgroundColor: 'rgba(26, 26, 46, 0.9)',
                  titleColor: '#f8fafc',
                  bodyColor: '#f8fafc',
                  borderColor: '#334155',
                  borderWidth: 1,
                  callbacks: {
                    label: function(context) {
                      return `${context.dataset.label}: ${formatCurrency(context.raw)}`;
                    }
                  }
                }
              },
              scales: {
                x: {
                  grid: {
                    color: 'rgba(255, 255, 255, 0.05)'
                  }
                },
                y: {
                  beginAtZero: true,
                  grid: {
                    color: 'rgba(255, 255, 255, 0.05)'
                  },
                  ticks: {
                    callback: function(value) {
                      return formatCurrency(value, false);
                    }
                  }
                }
              }
            }
          });
          
          updateCharts();
        } catch (error) {
          console.error("Error initializing charts:", error);
        }
      }
      
      function updateCharts() {
        try {
          // Update revenue chart
          if (revenueChart) {
            const months = getLastSixMonths();
            const revenueData = [];
            const cashData = [];
            
            months.forEach(month => {
              const [year, monthNum] = month.split('-');
              const monthProspects = prospects.filter(p => {
                if (!p.date) return false;
                const date = new Date(p.date);
                return date.getFullYear() === parseInt(year) && date.getMonth() === parseInt(monthNum) - 1;
              });
              
              const revenue = monthProspects.reduce((sum, p) => sum + (parseFloat(p.revenue) || 0), 0);
              const cash = monthProspects.reduce((sum, p) => sum + (parseFloat(p.cash) || 0), 0);
              
              revenueData.push(revenue);
              cashData.push(cash);
            });
            
            revenueChart.data.labels = months.map(formatMonthLabel);
            revenueChart.data.datasets[0].data = cashData;
            revenueChart.data.datasets[1].data = revenueData;
            revenueChart.update();
          }
        } catch (error) {
          console.error("Error updating charts:", error);
        }
      }
      
      function getLastSixMonths() {
        const months = [];
        const now = new Date();
        let year = now.getFullYear();
        let month = now.getMonth() + 1;
        
        for (let i = 0; i < 6; i++) {
          months.unshift(`${year}-${month.toString().padStart(2, '0')}`);
          month--;
          if (month === 0) {
            month = 12;
            year--;
          }
        }
        
        return months;
      }
      
      function formatMonthLabel(yearMonth) {
        const [year, month] = yearMonth.split('-');
        const date = new Date(parseInt(year), parseInt(month) - 1, 1);
        return date.toLocaleDateString(undefined, { month: 'short' });
      }
      
      // Update Sales Funnel
      function updateFunnel() {
        const container = document.getElementById('sales-funnel');
        container.innerHTML = '';
        
        // Get funnel data
        const totalLeads = prospects.length;
        const contactedLeads = prospects.filter(p => p.status === 'contacted' || p.status === 'scheduled' || p.status === 'closed' || p.status === 'noshow').length;
        const scheduledLeads = prospects.filter(p => p.status === 'scheduled' || p.status === 'closed' || p.status === 'noshow').length;
        const showedLeads = prospects.filter(p => p.status === 'closed' || p.status === 'contacted').length;
        const closedLeads = prospects.filter(p => p.status === 'closed').length;
        
        // Calculate percentages
        const stages = [
          { name: 'Total Leads', count: totalLeads, percentage: 100 },
          { name: 'Contacted', count: contactedLeads, percentage: totalLeads ? Math.round((contactedLeads / totalLeads) * 100) : 0 },
          { name: 'Scheduled', count: scheduledLeads, percentage: totalLeads ? Math.round((scheduledLeads / totalLeads) * 100) : 0 },
          { name: 'Showed Up', count: showedLeads, percentage: totalLeads ? Math.round((showedLeads / totalLeads) * 100) : 0 },
          { name: 'Closed', count: closedLeads, percentage: totalLeads ? Math.round((closedLeads / totalLeads) * 100) : 0 }
        ];
        
        // Create funnel steps
        stages.forEach(stage => {
          const stepDiv = document.createElement('div');
          stepDiv.className = 'funnel-step';
          
          const fillDiv = document.createElement('div');
          fillDiv.className = 'funnel-step-fill';
          fillDiv.style.width = '0%'; // Start at 0 for animation
          
          const contentDiv = document.createElement('div');
          contentDiv.className = 'funnel-step-content';
          
          const nameSpan = document.createElement('span');
          nameSpan.textContent = stage.name;
          
          const valueSpan = document.createElement('span');
          valueSpan.textContent = `${stage.count} (${stage.percentage}%)`;
          
          contentDiv.appendChild(nameSpan);
          contentDiv.appendChild(valueSpan);
          
          stepDiv.appendChild(fillDiv);
          stepDiv.appendChild(contentDiv);
          container.appendChild(stepDiv);
          
          // Animate fill after a short delay
          setTimeout(() => {
            fillDiv.style.width = `${stage.percentage}%`;
          }, 300);
        });
      }
      
      // Setters Performance functions
      function updateSettersPerformance() {
        // Process setters data
        const setters = processSettersData();
        updateSettersTable(setters);
        
        // Update top setters chart
        const topSetters = [...setters].sort((a, b) => b.callsSet - a.callsSet).slice(0, 5);
        updateTopPerformers(topSetters, 'top-setters', 'callsSet');
      }
      
      function processSettersData() {
        const setters = {};
        
        prospects.forEach(prospect => {
          if (!prospect.setby) return;
          
          const setter = prospect.setby;
          
          if (!setters[setter]) {
            setters[setter] = {
              name: setter,
              callsSet: 0,
              callsShowed: 0,
              noShows: 0,
              callsClosed: 0,
              cashCollected: 0,
              commission: 0
            };
          }
          
          // Count all leads set
          setters[setter].callsSet++;
          
          // Count shows and no-shows
          if (prospect.status === 'scheduled' || prospect.status === 'closed' || prospect.status === 'noshow') {
            if (prospect.status === 'noshow') {
              setters[setter].noShows++;
            } else if (prospect.status === 'closed' || prospect.status === 'contacted') {
              setters[setter].callsShowed++;
            }
          }
          
          // Count closed deals and cash
          if (prospect.status === 'closed') {
            setters[setter].callsClosed++;
            setters[setter].cashCollected += parseFloat(prospect.cash) || 0;
          }
          
          // Calculate commission (5% of cash collected for setters)
          setters[setter].commission = setters[setter].cashCollected * 0.05;
        });
        
        // Convert to array and sort by calls set
        return Object.values(setters).sort((a, b) => b.callsSet - a.callsSet);
      }
      
      function updateSettersTable(setters) {
        const tableBody = document.getElementById('setters-performance-body');
        tableBody.innerHTML = '';
        
        setters.forEach(setter => {
          const row = document.createElement('tr');
          
          // Name
          const nameCell = document.createElement('td');
          nameCell.textContent = setter.name;
          row.appendChild(nameCell);
          
          // Calls Set
          const callsSetCell = document.createElement('td');
          callsSetCell.textContent = setter.callsSet;
          row.appendChild(callsSetCell);
          
          // Show Rate
          const showRateCell = document.createElement('td');
          const scheduledCalls = setter.callsShowed + setter.noShows;
          const showRate = scheduledCalls ? ((setter.callsShowed / scheduledCalls) * 100).toFixed(1) : '0.0';
          showRateCell.textContent = `${showRate}%`;
          row.appendChild(showRateCell);
          
          // Calls Showed
          const callsShowedCell = document.createElement('td');
          callsShowedCell.textContent = setter.callsShowed;
          row.appendChild(callsShowedCell);
          
          // No Shows
          const noShowsCell = document.createElement('td');
          noShowsCell.textContent = setter.noShows;
          row.appendChild(noShowsCell);
          
          // Close Rate
          const closeRateCell = document.createElement('td');
          const closeRate = setter.callsSet ? ((setter.callsClosed / setter.callsSet) * 100).toFixed(1) : '0.0';
          closeRateCell.textContent = `${closeRate}%`;
          row.appendChild(closeRateCell);
          
          // Avg Deal Size
          const avgDealSizeCell = document.createElement('td');
          avgDealSizeCell.className = 'currency';
          const avgDealSize = setter.callsClosed ? setter.cashCollected / setter.callsClosed : 0;
          avgDealSizeCell.textContent = formatCurrency(avgDealSize);
          row.appendChild(avgDealSizeCell);
          
          // Commission
          const commissionCell = document.createElement('td');
          commissionCell.className = 'currency';
          commissionCell.textContent = formatCurrency(setter.commission);
          row.appendChild(commissionCell);
          
          tableBody.appendChild(row);
        });
      }
      
      // Closers Performance functions
      function updateClosersPerformance() {
        // Process closers data
        const closers = processClosersData();
        updateClosersTable(closers);
        
        // Update top closers chart
        const topClosers = [...closers].sort((a, b) => b.cashCollected - a.cashCollected).slice(0, 5);
        updateTopPerformers(topClosers, 'top-closers', 'cashCollected');
      }
      
      function processClosersData() {
        const closers = {};
        
        prospects.forEach(prospect => {
          // Skip if not closed and no closer assigned
          if (!prospect.closedby && prospect.status !== 'closed') return;
          
          // Use "Unassigned" for closed deals without a closer
          const closer = prospect.closedby || (prospect.status === 'closed' ? 'Unassigned' : null);
          if (!closer) return;
          
          if (!closers[closer]) {
            closers[closer] = {
              name: closer,
              callsTaken: 0,
              callsClosed: 0,
              cashCollected: 0,
              commission: 0
            };
          }
          
          // Count calls taken (all leads assigned to a closer)
          closers[closer].callsTaken++;
          
          // Count closed deals and cash
          if (prospect.status === 'closed') {
            closers[closer].callsClosed++;
            closers[closer].cashCollected += parseFloat(prospect.cash) || 0;
          }
          
          // Calculate commission (10% of cash collected for closers)
          closers[closer].commission = closers[closer].cashCollected * 0.10;
        });
        
        // Convert to array and sort by cash collected
        return Object.values(closers).sort((a, b) => b.cashCollected - a.cashCollected);
      }
      
      function updateClosersTable(closers) {
        const tableBody = document.getElementById('closers-performance-body');
        tableBody.innerHTML = '';
        
        closers.forEach(closer => {
          const row = document.createElement('tr');
          
          // Name
          const nameCell = document.createElement('td');
          nameCell.textContent = closer.name;
          row.appendChild(nameCell);
          
          // Calls Taken
          const callsTakenCell = document.createElement('td');
          callsTakenCell.textContent = closer.callsTaken;
          row.appendChild(callsTakenCell);
          
          // Calls Closed
          const callsClosedCell = document.createElement('td');
          callsClosedCell.textContent = closer.callsClosed;
          row.appendChild(callsClosedCell);
          
          // Close Rate
          const closeRateCell = document.createElement('td');
          const closeRate = closer.callsTaken ? ((closer.callsClosed / closer.callsTaken) * 100).toFixed(1) : '0.0';
          closeRateCell.textContent = `${closeRate}%`;
          row.appendChild(closeRateCell);
          
          // Cash Collected
          const cashCell = document.createElement('td');
          cashCell.className = 'currency';
          cashCell.textContent = formatCurrency(closer.cashCollected);
          row.appendChild(cashCell);
          
          // Cash Per Call
          const cashPerCallCell = document.createElement('td');
          cashPerCallCell.className = 'currency';
          const cashPerCall = closer.callsTaken ? closer.cashCollected / closer.callsTaken : 0;
          cashPerCallCell.textContent = formatCurrency(cashPerCall);
          row.appendChild(cashPerCallCell);
          
          // Avg Deal Size
          const avgDealSizeCell = document.createElement('td');
          avgDealSizeCell.className = 'currency';
          const avgDealSize = closer.callsClosed ? closer.cashCollected / closer.callsClosed : 0;
          avgDealSizeCell.textContent = formatCurrency(avgDealSize);
          row.appendChild(avgDealSizeCell);
          
          // Commission
          const commissionCell = document.createElement('td');
          commissionCell.className = 'currency';
          commissionCell.textContent = formatCurrency(closer.commission);
          row.appendChild(commissionCell);
          
          tableBody.appendChild(row);
        });
      }
      
      // Generic function to update top performers chart
      function updateTopPerformers(performers, containerId, metricKey) {
        const container = document.getElementById(containerId);
        container.innerHTML = '';
        
        // Find max value for scaling
        const maxValue = Math.max(...performers.map(p => p[metricKey]), 1); // Avoid division by zero
        
        performers.forEach((performer, index) => {
          const barHeight = Math.max((performer[metricKey] / maxValue * 100), 5); // Min 5% height for visibility
          
          const performerDiv = document.createElement('div');
          performerDiv.className = 'performer-bar';
          
          const bar = document.createElement('div');
          bar.className = 'bar';
          bar.style.height = '0px'; // Start at 0 for animation
          
          const nameDiv = document.createElement('div');
          nameDiv.className = 'performer-name';
          nameDiv.textContent = performer.name;
          
          const valueDiv = document.createElement('div');
          valueDiv.className = 'performer-value';
          valueDiv.textContent = metricKey === 'cashCollected' ? 
            formatCurrency(performer[metricKey]) : 
            performer[metricKey];
          
          performerDiv.appendChild(bar);
          performerDiv.appendChild(nameDiv);
          performerDiv.appendChild(valueDiv);
          
          container.appendChild(performerDiv);
          
          // Animate the bar height after a short delay
          setTimeout(() => {
            bar.style.height = `${barHeight}%`;
          }, 100 * index); // Stagger the animations
        });
        
        // Add empty bars if less than 5 performers
        for (let i = performers.length; i < 5; i++) {
          const emptyDiv = document.createElement('div');
          emptyDiv.className = 'performer-bar';
          
          const emptyBar = document.createElement('div');
          emptyBar.className = 'bar';
          emptyBar.style.height = '0px';
          emptyBar.style.opacity = '0.2';
          
          const emptyName = document.createElement('div');
          emptyName.className = 'performer-name';
          emptyName.textContent = '—';
          emptyName.style.opacity = '0.5';
          
          const emptyValue = document.createElement('div');
          emptyValue.className = 'performer-value';
          emptyValue.textContent = metricKey === 'cashCollected' ? '$0' : '0';
          emptyValue.style.opacity = '0.5';
          
          emptyDiv.appendChild(emptyBar);
          emptyDiv.appendChild(emptyName);
          emptyDiv.appendChild(emptyValue);
          
          container.appendChild(emptyDiv);
        }
      }
      
      function openAddModal() {
        document.getElementById('modal-title').textContent = 'Edit Lead';
        document.getElementById('lead-id').value = id;
        currentEditId = id;
        document.getElementById('prospect-name').value = prospect.name || '';
        document.getElementById('date-in').value = prospect.date || '';
        document.getElementById('call-outcome').value = prospect.status || '';
        document.getElementById('lead-source').value = prospect.source || '';
        document.getElementById('set-by').value = prospect.setby || '';
        document.getElementById('closed-by').value = prospect.closedby || '';
        document.getElementById('cash-collected').value = prospect.cash || 0;
        document.getElementById('revenue-generated').value = prospect.revenue || 0;
        document.getElementById('notes').value = prospect.notes || '';
        
        openModal('lead-modal');
      }

      function openDeleteModal(id) {
        currentEditId = id;
        openModal('delete-modal');
      }

      function openModal(modalId) {
        const modal = document.getElementById(modalId);
        modal.style.display = 'block';
        setTimeout(() => {
          modal.classList.add('show');
        }, 10);
      }

      function closeModal(modalId) {
        const modal = document.getElementById(modalId);
        modal.classList.remove('show');
        setTimeout(() => {
          modal.style.display = 'none';
        }, 300);
      }

      function setupEventListeners() {
        // Add Lead Button
        document.getElementById('add-lead-btn').addEventListener('click', openAddModal);
        document.getElementById('empty-add-btn').addEventListener('click', openAddModal);
        
        // Form Submit
        document.getElementById('lead-form').addEventListener('submit', function(e) {
          e.preventDefault();
          
          const id = document.getElementById('lead-id').value;
          const lead = {
            name: document.getElementById('prospect-name').value,
            date: document.getElementById('date-in').value,
            status: document.getElementById('call-outcome').value,
            source: document.getElementById('lead-source').value,
            cash: document.getElementById('cash-collected').value,
            revenue: document.getElementById('revenue-generated').value,
            setby: document.getElementById('set-by').value,
            closedby: document.getElementById('closed-by').value,
            notes: document.getElementById('notes').value
          };
          
          if (id) {
            updateLead(id, lead);
          } else {
            addLead(lead);
          }
          
          closeModal('lead-modal');
        });
        
        // Modal Close Buttons
        document.getElementById('close-modal').addEventListener('click', () => closeModal('lead-modal'));
        document.getElementById('cancel-btn').addEventListener('click', () => closeModal('lead-modal'));
        document.getElementById('close-delete-modal').addEventListener('click', () => closeModal('delete-modal'));
        document.getElementById('cancel-delete-btn').addEventListener('click', () => closeModal('delete-modal'));
        
        // Delete Confirmation
        document.getElementById('confirm-delete-btn').addEventListener('click', function() {
          if (currentEditId) {
            deleteLead(currentEditId);
            closeModal('delete-modal');
          }
        });
        
        // View Filters
        document.querySelectorAll('.view-btn').forEach(btn => {
          btn.addEventListener('click', function() {
            document.querySelectorAll('.view-btn').forEach(b => b.classList.remove('active'));
            this.classList.add('active');
            currentView = this.dataset.view;
            renderTable();
          });
        });
        
        // Search Input
        document.getElementById('search-input').addEventListener('input', function() {
          searchTerm = this.value.trim();
          renderTable();
        });
        
        // Dropdown Menu Toggle
        document.getElementById('dropdown-btn').addEventListener('click', function() {
          document.getElementById('dropdown-menu').classList.toggle('show');
        });
        
        // Close the dropdown when clicking outside
        window.addEventListener('click', function(event) {
          if (!event.target.matches('#dropdown-btn')) {
            const dropdowns = document.getElementsByClassName('dropdown-content');
            for (let i = 0; i < dropdowns.length; i++) {
              const openDropdown = dropdowns[i];
              if (openDropdown.classList.contains('show')) {
                openDropdown.classList.remove('show');
              }
            }
          }
        });
        
        // Export to CSV
        document.getElementById('export-btn').addEventListener('click', exportToCSV);
        
        // Clear All Data
        document.getElementById('clear-btn').addEventListener('click', function() {
          if (confirm('Are you sure you want to delete ALL data? This cannot be undone!')) {
            prospects = [];
            saveData();
            renderTable();
            updateAllMetrics();
            updateCharts();
            updateSettersPerformance();
            updateClosersPerformance();
            updateFunnel();
          }
        });
        
        // Sortable Table Headers
        document.querySelectorAll('th.sortable').forEach(th => {
          th.addEventListener('click', function() {
            const field = this.dataset.sort;
            if (currentSort.field === field) {
              currentSort.direction = currentSort.direction === 'asc' ? 'desc' : 'asc';
            } else {
              currentSort.field = field;
              currentSort.direction = 'asc';
            }
            
            // Update the UI
            document.querySelectorAll('th.sortable').forEach(el => {
              el.classList.remove('asc', 'desc');
            });
            this.classList.add(currentSort.direction);
            
            renderTable();
          });
        });
      }

      // Export to CSV Function
      function exportToCSV() {
        if (prospects.length === 0) {
          alert('No data to export');
          return;
        }
        
        const filteredProspects = filterProspects();
        
        let csv = 'Name,Date,Status,Source,Set By,Closed By,Cash Collected,Revenue,Notes\n';
        
        filteredProspects.forEach(p => {
          const row = [
            '"' + (p.name || '').replace(/"/g, '""') + '"',
            '"' + (p.date || '') + '"',
            '"' + (p.status || '') + '"',
            '"' + (p.source || '').replace(/"/g, '""') + '"',
            '"' + (p.setby || '').replace(/"/g, '""') + '"',
            '"' + (p.closedby || '').replace(/"/g, '""') + '"',
            p.cash || 0,
            p.revenue || 0,
            '"' + (p.notes || '').replace(/"/g, '""') + '"'
          ];
          csv += row.join(',') + '\n';
        });
        
        const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
        const url = URL.createObjectURL(blob);
        
        const link = document.createElement('a');
        link.setAttribute('href', url);
        link.setAttribute('download', 'sales_data_export.csv');
        link.style.visibility = 'hidden';
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
      }

      // Currency formatting helper
      function formatCurrency(value, includeSymbol = true) {
        const amount = parseFloat(value) || 0;
        return includeSymbol 
          ? '$' + amount.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 })
          : amount.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
      }
    });
  </script>
</body>
</html>
