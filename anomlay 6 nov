<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DPN Anomaly Detection - Production v12.0</title>
    <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🚀</text></svg>">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body { background-color: #f8f9fa; font-size: 0.95rem; }
        .header { 
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); 
            color: white; 
            padding: 25px; 
            text-align: center; 
            margin-bottom: 20px; 
            box-shadow: 0 4px 6px rgba(0,0,0,0.1); 
        }
        .panel { 
            background-color: white; 
            padding: 25px; 
            border-radius: 12px; 
            box-shadow: 0 4px 12px rgba(0,0,0,0.08); 
            min-height: 75vh; 
            height: 100%;
        }
        .sortable-header { 
            cursor: pointer; 
            user-select: none; 
            transition: background-color 0.2s; 
        }
        .sortable-header:hover { background-color: #e9ecef; }
        .page-link { cursor: pointer; }
        .filter-panel { 
            background-color: #f8f9fa; 
            border: 1px solid #e9ecef; 
            border-radius: .5rem; 
            padding: 1rem; 
        }
        .multi-select-dropdown { position: relative; }
        .multi-select-button { 
            width: 100%; 
            font-size: .875rem; 
            text-align: left; 
            background-color: white; 
            border: 1px solid #ced4da; 
            padding: .35rem .75rem; 
            border-radius: .375rem; 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
            cursor: pointer; 
            transition: all 0.2s; 
        }
        .multi-select-button:hover { 
            background-color: #f8f9fa; 
            border-color: #86b7fe; 
        }
        .multi-select-button::after { 
            content: "▼"; 
            font-size: 0.7rem; 
            color: #6c757d; 
            margin-left: .5rem; 
        }
        .multi-select-menu { 
            display: none; 
            position: absolute; 
            width: 100%; 
            max-height: 280px; 
            overflow-y: auto; 
            background-color: white; 
            border: 1px solid #ced4da; 
            border-radius: .5rem; 
            z-index: 1050; 
            padding: .5rem; 
            box-shadow: 0 8px 24px rgba(0,0,0,0.15); 
            margin-top: 4px; 
        }
        .multi-select-menu.show { 
            display: block; 
            animation: slideDown 0.2s ease-out; 
        }
        @keyframes slideDown { 
            from { opacity: 0; transform: translateY(-10px); } 
            to { opacity: 1; transform: translateY(0); } 
        }
        .multi-select-option { 
            padding: .5rem; 
            cursor: default;
            border-radius: .375rem; 
            display: flex; 
            align-items: center; 
        }
        .multi-select-option input[type="checkbox"] { 
            margin-right: .6rem; 
            cursor: pointer;
        }
        .multi-select-option label {
            cursor: pointer;
            margin: 0;
            flex: 1;
        }
        .multi-select-option.all-option { 
            font-weight: 600; 
            border-bottom: 1px solid #dee2e6; 
            margin-bottom: .3rem; 
            padding-bottom: .6rem; 
        }
        .form-check { cursor: default; }
        .form-check-input { cursor: pointer; }
        .form-check-label { cursor: pointer; }
        .selected-count { 
            color: #0d6efd; 
            font-weight: 600; 
            font-size: 0.85rem; 
        }
        .classified-row { background-color: #d1e7dd !important; }
        .classified-row td { color: #0a3622; font-weight: 500; }
        .closed-row { background-color: #cfe2ff !important; }
        .closed-row td { color: #052c65; font-weight: 500; }
        .loading-overlay { 
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: rgba(0,0,0,0.7); display: flex; flex-direction: column; 
            justify-content: center; align-items: center; z-index: 9999; 
        }
        .loading-overlay .spinner-border { width: 3rem; height: 3rem; }
        .loading-overlay p { color: white; margin-top: 1rem; font-size: 1.1rem; }
        .toast-container { position: fixed; top: 20px; right: 20px; z-index: 9998; }
        .sr-only { 
            position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; 
            overflow: hidden; clip: rect(0,0,0,0); white-space: nowrap; border-width: 0; 
        }
        .export-buttons .btn { margin-left: 0.5rem; }
        .path-badge {
            background: #0d6efd; color: white; padding: .25rem .75rem;
            border-radius: .375rem; font-family: monospace; font-size: .9rem;
        }
        .week-badge {
            background: #198754; color: white; padding: .25rem .5rem;
            border-radius: .25rem; margin: .15rem; display: inline-block; font-size: .85rem;
        }
        .confirmation-modal, .irev-modal {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.7); display: flex;
            justify-content: center; align-items: center; z-index: 10000;
            animation: fadeIn 0.2s ease-out;
        }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .confirmation-content, .irev-content {
            background: white; padding: 2rem; border-radius: .75rem;
            width: 90%; max-width: 600px; box-shadow: 0 10px 40px rgba(0,0,0,0.3);
            transform: scale(0.95);
            animation: scaleIn 0.2s ease-out forwards;
            max-height: 90vh;
            overflow-y: auto;
        }
        @keyframes scaleIn { from { transform: scale(0.95); opacity: 0; } to { transform: scale(1); opacity: 1; } }
        .irev-content { max-width: 500px; }
        .warning-icon { font-size: 4rem; color: #dc3545; text-align: center; margin-bottom: 1rem; }
        .visitor-id-link { color: #6f42c1; font-weight: 600; cursor: pointer; text-decoration: none; }
        .visitor-id-link:hover { color: #59369a; text-decoration: underline; }
        .info-box {
            background-color: #e7f3fe; border-left: 5px solid #0d6efd;
            padding: 15px; margin: 20px 0; font-size: 1em; border-radius: 4px;
        }
        .control-section {
            border: 1px solid #dee2e6; border-radius: .5rem;
            padding: 1.5rem; background-color: #f8f9fa; height: 100%;
        }
        .chart-container {
            position: relative; margin: auto; height: 40vh; width: 100%;
        }
        .irev-hint {
            font-size: 0.75rem;
            color: #6c757d;
            font-style: italic;
            display: block;
            margin-top: 0.25rem;
        }
        .validation-error {
            color: #dc3545;
            font-size: 0.875rem;
            margin-top: 0.25rem;
            display: none;
        }
        .validation-error.show {
            display: block;
        }

        /* Responsive Enhancements */
        @media (max-width: 768px) {
            .header h1 { font-size: 1.5rem; }
            .header .lead { font-size: 0.9rem; }
            .panel { padding: 15px; min-height: auto; }
            .filter-panel { padding: 0.75rem; }
            .filter-panel .col-md-2 { margin-bottom: 1rem; }
            .export-buttons { display: flex; flex-direction: column; gap: 0.5rem; }
            .export-buttons .btn { margin-left: 0; width: 100%; }
            .control-section { padding: 1rem; margin-bottom: 1rem; }
            .confirmation-content, .irev-content { padding: 1.5rem; width: 95%; }
            .warning-icon { font-size: 2.5rem; }
            .chart-container { height: 30vh; }
            .table { font-size: 0.85rem; }
            .path-badge { font-size: 0.75rem; padding: 0.2rem 0.5rem; }
            .week-badge { font-size: 0.75rem; }
        }

        @media (max-width: 576px) {
            .nav-tabs .nav-link { font-size: 0.85rem; padding: 0.5rem 0.75rem; }
            .table-responsive { font-size: 0.8rem; }
        }

        /* Tablet responsive */
        @media (min-width: 768px) and (max-width: 1024px) {
            .filter-panel .col-md-2 { flex: 0 0 50%; max-width: 50%; }
            .chart-container { height: 35vh; }
        }
    </style>
</head>
<body>
    <div id="loading-overlay" class="loading-overlay" style="display: none;">
        <div class="spinner-border text-light" role="status">
            <span class="visually-hidden">Loading...</span>
        </div>
        <p id="loading-message">Processing...</p>
    </div>

    <div class="toast-container" id="toast-container"></div>

    <div class="header">
        <h1>🚀 DPN Anomaly Detection</h1>
        <p class="lead mb-0">Fully Automated Anomaly Investigation & Analysis</p>
    </div>

    <div class="container-fluid px-2 px-md-4">
        <ul class="nav nav-tabs mb-3" id="mainTab" role="tablist">
            <li class="nav-item" role="presentation">
                <button class="nav-link active" id="explorer-tab" data-bs-toggle="tab" data-bs-target="#explorer-pane" type="button" role="tab">Anomaly Inventory</button>
            </li>
            <li class="nav-item" role="presentation">
                <button class="nav-link" id="granular-tab" data-bs-toggle="tab" data-bs-target="#granular-pane" type="button" role="tab">Illustrative Cases</button>
            </li>
            <li class="nav-item" role="presentation">
                <button class="nav-link" id="session-analysis-tab-btn" data-bs-toggle="tab" data-bs-target="#session-analysis-pane" type="button" role="tab">Session Analysis</button>
            </li>
            <li class="nav-item" role="presentation">
                <button class="nav-link" id="log-tab" data-bs-toggle="tab" data-bs-target="#log-pane" type="button" role="tab">Investigation Log</button>
            </li>
            <li class="nav-item" role="presentation">
                <button class="nav-link" id="analytics-tab" data-bs-toggle="tab" data-bs-target="#analytics-pane" type="button" role="tab">Outcome Analytics</button>
            </li>
        </ul>

        <div class="tab-content" id="mainTabContent">
            <div class="tab-pane fade show active" id="explorer-pane" role="tabpanel">
                <div class="row g-2 g-md-4">
                    <div class="col-12 col-lg-12">
                        <div class="panel">
                            <div class="d-flex justify-content-end align-items-start mb-3">
                                <div class="export-buttons">
                                    <button class="btn btn-sm btn-outline-danger" id="reset-all-filters-btn" title="Reset grid filters">🔄 Reset Filters</button>
                                    <button class="btn btn-sm btn-outline-success" id="export-csv-btn" title="Export to CSV">📊 Export CSV</button>
                                    <button class="btn btn-sm btn-outline-primary" id="export-json-btn" title="Export to JSON">📄 Export JSON</button>
                                </div>
                            </div>

                            <div id="details-container">
                                <div class="text-center p-5">
                                    <p class="text-muted">Loading anomaly data...</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="tab-pane fade" id="granular-pane" role="tabpanel">
                <div class="panel" style="max-height: 80vh; overflow-y: auto;">
                    <div id="granular-details-content"><div class="text-center p-5"><p class="text-muted">Click a row to view illustrative cases.</p></div></div>
                </div>
            </div>

            <div class="tab-pane fade" id="session-analysis-pane" role="tabpanel">
                <div class="panel" style="max-height: 80vh; overflow-y: auto;"><div id="session-analysis-content"></div></div>
            </div>

            <div class="tab-pane fade" id="log-pane" role="tabpanel">
                <div class="panel">
                    <h4 class="mb-3">Investigation Log</h4>
                    <div class="table-responsive">
                        <table class="table table-striped table-hover">
                            <thead id="log-table-head"></thead>
                            <tbody id="log-table-body"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div class="tab-pane fade" id="analytics-pane" role="tabpanel">
                <div class="panel">
                    <h4 class="mb-3">Outcome Analytics Dashboard</h4>
                    <div id="analytics-content"><div class="text-center p-5"><p class="text-muted">No classification data available.</p></div></div>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    
    <script>
    'use strict';

    // ==================== CONFIGURATION ====================
    const CONFIG = {
        ROWS_PER_PAGE: 10,
        FILTERABLE_COLUMNS: ["Journey Type", "Team", "Business Unit", "Device", "Completed", "Hard Anomaly"],
        CLASSIFICATION_CATEGORIES: ["Technical Error", "Previously Anomalies Fixed", "New Tag/Page", "Limited-time-offers (LTO)", "Marketing Change", "Minor Anomalies", "False Positives", "Past Changes", "Unknown Causes", "Missing Tags"],
        RESPONSIBLE_TEAMS: ["tech-team-alpha@company.com", "engineering-prod-support@company.com", "marketing-analytics@company.com", "data-integrity-team@company.com", "platform-services@company.com"],
        DISPLAY_COLUMNS: ["Row ID", "Week", "Path", "Journey Type", "Team", "Business Unit", "Device", "Completed", "Hard Anomaly", "Anomaly Type", "iREV Value ($)", "Team Responsible to Fix", "Resolution Date"],
        LOG_DISPLAY_COLUMNS: ["Row ID", "Week", "Path", "Anomaly Type", "iREV Value ($)", "Investigation Timestamp", "Team Responsible", "Resolution Date", "Resolution Timestamp"],
        DEBOUNCE_DELAY: 150,
        IREV_MAX_DECIMALS: 5,
        IREV_DEFAULT_VALUE: '0.0000'
    };

    // ==================== PERFORMANCE UTILITIES ====================
    class PerformanceOptimizer {
        static debounce(func, delay) {
            let timeoutId;
            return function(...args) {
                clearTimeout(timeoutId);
                timeoutId = setTimeout(() => func.apply(this, args), delay);
            };
        }
    }

    // ==================== VALIDATION UTILITIES ====================
    class ValidationUtils {
        static validateIrevValue(value) {
            if (value === null || value === undefined || value.trim() === '') {
                return { valid: true, value: CONFIG.IREV_DEFAULT_VALUE };
            }
            
            const numValue = parseFloat(value);
            if (isNaN(numValue) || numValue < 0) {
                return { valid: false, error: 'iREV value must be a positive number' };
            }
            
            const decimalMatch = value.match(/\.(\d+)$/);
            if (decimalMatch && decimalMatch[1].length > CONFIG.IREV_MAX_DECIMALS) {
                return { 
                    valid: false, 
                    error: `iREV value cannot have more than ${CONFIG.IREV_MAX_DECIMALS} decimal places` 
                };
            }
            
            return { valid: true, value: numValue.toFixed(4) };
        }

        static validateResolutionDate(dateString) {
            if (!dateString) {
                return { valid: false, error: 'Resolution date is required' };
            }
            
            const date = new Date(dateString);
            const year = date.getFullYear();
            
            if (year < 1000 || year > 9999) {
                return { valid: false, error: 'Year must be between 1000 and 9999' };
            }
            
            return { valid: true, value: dateString };
        }

        static showFieldError(fieldId, errorMessage) {
            const field = document.getElementById(fieldId);
            const errorDiv = document.getElementById(`${fieldId}-error`);
            
            if (field) {
                field.classList.add('is-invalid');
            }
            
            if (errorDiv) {
                errorDiv.textContent = errorMessage;
                errorDiv.classList.add('show');
            }
        }

        static clearFieldError(fieldId) {
            const field = document.getElementById(fieldId);
            const errorDiv = document.getElementById(`${fieldId}-error`);
            
            if (field) {
                field.classList.remove('is-invalid');
            }
            
            if (errorDiv) {
                errorDiv.textContent = '';
                errorDiv.classList.remove('show');
            }
        }
    }

    // ==================== MEMOIZATION CACHE ====================
    class MemoizationCache {
        constructor(maxSize = 1000) { this.cache = new Map(); this.maxSize = maxSize; }
        get(key) { return this.cache.get(key); }
        set(key, value) {
            if (this.cache.size >= this.maxSize) { this.cache.delete(this.cache.keys().next().value); }
            this.cache.set(key, value);
        }
        has(key) { return this.cache.has(key); }
        clear() { this.cache.clear(); }
    }

    // ==================== UTILITY FUNCTIONS ====================
    function escapeHtml(unsafe) {
        if (unsafe === null || unsafe === undefined) return '';
        const div = document.createElement('div');
        div.textContent = String(unsafe);
        return div.innerHTML;
    }
    
    function formatTimestamp(date) {
        const d = new Date(date);
        const year = d.getFullYear();
        const month = String(d.getMonth() + 1).padStart(2, '0');
        const day = String(d.getDate()).padStart(2, '0');
        const hours = String(d.getHours()).padStart(2, '0');
        const minutes = String(d.getMinutes()).padStart(2, '0');
        const seconds = String(d.getSeconds()).padStart(2, '0');
        return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
    }

    // ==================== UI HELPERS ====================
    class UIHelpers {
        static showLoading(message = 'Processing...') {
            const overlay = document.getElementById('loading-overlay');
            const messageEl = document.getElementById('loading-message');
            messageEl.textContent = message;
            overlay.style.display = 'flex';
            this.announceToScreenReader(message);
        }
        static hideLoading() { document.getElementById('loading-overlay').style.display = 'none'; }
        
        static showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            const toastId = 'toast-' + Date.now();
            const bgClass = { success: 'bg-success', error: 'bg-danger', warning: 'bg-warning text-dark', info: 'bg-info' }[type] || 'bg-success';
            
            const toast = document.createElement('div');
            toast.id = toastId;
            toast.className = `toast align-items-center text-white ${bgClass} border-0`;
            toast.setAttribute('role', 'alert');
            toast.setAttribute('aria-live', 'assertive');
            toast.setAttribute('aria-atomic', 'true');
            toast.innerHTML = `<div class="d-flex"><div class="toast-body">${escapeHtml(message)}</div><button type="button" class="btn-close ${type === 'warning' ? '' : 'btn-close-white'} me-2 m-auto" data-bs-dismiss="toast"></button></div>`;
            
            container.appendChild(toast);
            const bsToast = new bootstrap.Toast(toast, { delay: 4000 });
            bsToast.show();
            toast.addEventListener('hidden.bs.toast', () => toast.remove());
            this.announceToScreenReader(message);
        }
        
        static announceToScreenReader(message) {
            const announcement = document.createElement('div');
            announcement.setAttribute('role', 'status');
            announcement.setAttribute('aria-live', 'polite');
            announcement.className = 'sr-only';
            announcement.textContent = message;
            document.body.appendChild(announcement);
            setTimeout(() => announcement.remove(), 1000);
        }
        
        static async showConfirmationDialog(options) {
            return new Promise((resolve) => {
                const modal = document.createElement('div');
                modal.className = 'confirmation-modal';
                
                let classificationInfo = `<strong>Anomaly Type:</strong> <span class="badge bg-primary">${escapeHtml(options.classification)}</span><br><br>`;
                let resolutionInfo = options.teamResponsible ? `<strong>Team Responsible:</strong> ${escapeHtml(options.teamResponsible)}<br><br><strong>Resolution Date:</strong> ${escapeHtml(options.resolutionDate)}<br><br>` : '';

                modal.innerHTML = `
                    <div class="confirmation-content">
                        <div class="warning-icon">⚠️</div>
                        <h3 class="text-center text-danger mb-3">WARNING</h3>
                        <h5 class="text-center mb-4">${escapeHtml(options.title)}</h5>
                        <div class="alert alert-danger mb-4">
                            <strong>Path:</strong> <span class="path-badge">${escapeHtml(options.path)}</span><br><br>
                            ${classificationInfo}
                            <strong>Week Date(s):</strong><br>${options.weeks.map(w => `<span class="week-badge">${escapeHtml(w)}</span>`).join('')}<br><br>
                            <strong>Affected Rows:</strong> ${options.affectedRows}<br><br>
                            ${options.irev ? `<strong>iREV Value ($):</strong> ${escapeHtml(options.irev)}<br><br>` : ''}
                            ${resolutionInfo}
                            <div class="text-center mt-3 fw-bold text-uppercase">⚠️ THIS ACTION CANNOT BE UNDONE ⚠️</div>
                        </div>
                        <div class="d-flex justify-content-between gap-3 flex-wrap">
                            <button class="btn btn-secondary btn-lg flex-fill" id="cancel-action">Cancel</button>
                            <button class="btn btn-danger btn-lg flex-fill" id="confirm-action">${escapeHtml(options.confirmText)}</button>
                        </div>
                    </div>`;
                
                document.body.appendChild(modal);
                document.getElementById('cancel-action').addEventListener('click', () => { modal.remove(); resolve(false); });
                document.getElementById('confirm-action').addEventListener('click', () => { modal.remove(); resolve(true); });
            });
        }
    }

    // ==================== DATA EXPORT ====================
    class DataExporter {
        static exportToCSV(data, filename) {
            if (!data || data.length === 0) return UIHelpers.showToast('No data to export', 'warning');
            const headers = Object.keys(data[0]);
            const csv = [ headers.join(','), ...data.map(row => headers.map(h => this.escapeCSV(row[h])).join(',')) ].join('\n');
            this.downloadFile(csv, filename, 'text/csv');
            UIHelpers.showToast(`Exported ${data.length} records to CSV`, 'success');
        }
        
        static exportToJSON(data, filename) {
            if (!data || data.length === 0) return UIHelpers.showToast('No data to export', 'warning');
            const json = JSON.stringify(data, null, 2);
            this.downloadFile(json, filename, 'application/json');
            UIHelpers.showToast(`Exported ${data.length} records to JSON`, 'success');
        }
        
        static escapeCSV(value) {
            if (value === null || value === undefined) return '';
            const str = String(value);
            return (str.includes(',') || str.includes('"') || str.includes('\n')) ? `"${str.replace(/"/g, '""')}"` : str;
        }
        
        static downloadFile(content, filename, mimeType) {
            const blob = new Blob([content], { type: mimeType });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = filename;
            a.click();
            URL.revokeObjectURL(url);
        }
    }

    // ==================== SAMPLE DATA ====================
    const SAMPLE_DATA = [
        { "Row ID": 'a1', "Week": '2024-01-15', "Path": '/home', "Journey Type": "Basic", "Team": "US", "Business Unit": "Consumer", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [ { "Visitor Id": "1234567891524_987654321124356A", "Visitor No": 1, "Visitor Page No": [3, 4, 8], "MCV ID": "MCVID_ABC123", "Agent ID": "AG_001", "Correlation ID": "CORR_XYZ789", "URL Referral": "https://www.americanexpress.com/search?q=product", "ContentSquare URL Link": "https://contentsquare.com/session/abc123" }, { "Visitor Id": "1234567891524_987654321124356A", "Visitor No": 2, "Visitor Page No": [1, 5], "MCV ID": "MCVID_ABC123", "Agent ID": "AG_001", "Correlation ID": "CORR_XYZ790", "URL Referral": "https://www.google.com", "ContentSquare URL Link": "https://contentsquare.com/session/abc123" } ] },
        { "Row ID": 'a2', "Week": '2024-01-15', "Path": '/home', "Journey Type": "Upgraded", "Team": "US", "Business Unit": "PL", "Device": "PC", "Completed": "no", "Hard Anomaly": "true", "granular_details": [ { "Visitor Id": "2345678912635_876543212235467B", "Visitor No": 1, "Visitor Page No": [2, 5], "MCV ID": "MCVID_DEF456", "Agent ID": "AG_003", "Correlation ID": "CORR_ABC001", "URL Referral": "https://www.americanexpress.com/posts/tech", "ContentSquare URL Link": "https://contentsquare.com/session/def456" } ] },
        { "Row ID": 'a3', "Week": '2024-01-15', "Path": '/home', "Journey Type": "Basic", "Team": "India", "Business Unit": "Consumer", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [ { "Visitor Id": "3456789123746_765432133465788C", "Visitor No": 1, "Visitor Page No": [1, 3], "MCV ID": "MCVID_GHI789", "Agent ID": "AG_004", "Correlation ID": "CORR_ABC002", "URL Referral": "https://www.americanexpress.com/ad/promo", "ContentSquare URL Link": "https://contentsquare.com/session/ghi789" }, { "Visitor Id": "3456789123746_765432133465788C", "Visitor No": 2, "Visitor Page No": [2, 4, 6], "MCV ID": "MCVID_GHI789", "Agent ID": "AG_004", "Correlation ID": "CORR_ABC003", "URL Referral": "https://www.bing.com", "ContentSquare URL Link": "https://contentsquare.com/session/ghi789" }, { "Visitor Id": "3456789123746_765432133465788C", "Visitor No": 3, "Visitor Page No": [1], "MCV ID": "MCVID_GHI789", "Agent ID": "AG_004", "Correlation ID": "CORR_ABC004", "URL Referral": "https://www.amex.com", "ContentSquare URL Link": "https://contentsquare.com/session/ghi789" } ] },
        { "Row ID": 'a4', "Week": '2024-01-15', "Path": '/home', "Journey Type": null, "Team": "US", "Business Unit": "sbs", "Device": "desktop", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [ { "Visitor Id": "4567891234857_654321445768999D", "Visitor No": 1, "Visitor Page No": [10, 15], "MCV ID": "MCVID_JKL012", "Agent ID": "AG_005", "Correlation ID": "CORR_ABC004", "URL Referral": "https://www.americanexpress.com/r/technology", "ContentSquare URL Link": "https://contentsquare.com/session/jkl012" } ] },
        { "Row ID": 'a5', "Week": '2024-01-15', "Path": '/home', "Journey Type": "Upgraded", "Team": "India", "Business Unit": "PL", "Device": null, "Completed": "no", "Hard Anomaly": "true", "granular_details": [ { "Visitor Id": "5678912345968_543215566871234E", "Visitor No": 1, "Visitor Page No": [5, 10], "MCV ID": "MCVID_MNO345", "Agent ID": "AG_006", "Correlation ID": "CORR_ABC005", "URL Referral": "https://www.americanexpress.com/ad/story", "ContentSquare URL Link": "https://contentsquare.com/session/mno345" } ] },
        { "Row ID": 'a6', "Week": '2024-01-15', "Path": '/home', "Journey Type": "Basic", "Team": "US", "Business Unit": "Consumer", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [ { "Visitor Id": "6789123456079_432166779823456F", "Visitor No": 1, "Visitor Page No": [1, 2], "MCV ID": "MCVID_PQR678", "Agent ID": "AG_007", "Correlation ID": "CORR_ABC006", "URL Referral": "https://www.americanexpress.com", "ContentSquare URL Link": "https://contentsquare.com/session/pqr678" } ] },
        { "Row ID": 'b1', "Week": '2024-01-22', "Path": '/products', "Journey Type": "Basic", "Team": "US", "Business Unit": "Consumer", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [{ "Visitor Id": "7891234567180_321778881934567G", "Visitor No": 1, "Visitor Page No": [3, 6], "MCV ID": "MCVID_STU901", "Agent ID": "AG_001", "Correlation ID": "CORR_XYZ801", "URL Referral": "https://www.americanexpress.com/search", "ContentSquare URL Link": "https://contentsquare.com/session/stu901" }] },
        { "Row ID": 'b2', "Week": '2024-01-22', "Path": '/products', "Journey Type": "Upgraded", "Team": "India", "Business Unit": "PL", "Device": "PC", "Completed": "no", "Hard Anomaly": "true", "granular_details": [{ "Visitor Id": "V00202", "Visitor No": 1, "Visitor Page No": [2, 8], "MCV ID": "MCVID_VWX234", "Agent ID": "AG_002", "Correlation ID": "CORR_ABC101", "URL Referral": "https://www.americanexpress.com/posts/article", "ContentSquare URL Link": "https://contentsquare.com/session/vwx234" }] },
        { "Row ID": 'b3', "Week": '2024-01-22', "Path": '/products', "Journey Type": "Basic", "Team": "US", "Business Unit": "Consumer", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [{ "Visitor Id": "V00203", "Visitor No": 1, "Visitor Page No": [1, 4], "MCV ID": "MCVID_YZA567", "Agent ID": "AG_003", "Correlation ID": "CORR_ABC102", "URL Referral": "https://www.americanexpress.com/campaign", "ContentSquare URL Link": "https://contentsquare.com/session/yza567" }] },
        { "Row ID": 'b4', "Week": '2024-01-22', "Path": '/products', "Journey Type": null, "Team": "India", "Business Unit": "sbs", "Device": "desktop", "Completed": "no", "Hard Anomaly": "true", "granular_details": [{ "Visitor Id": "V00204", "Visitor No": 1, "Visitor Page No": [5, 12], "MCV ID": "MCVID_BCD890", "Agent ID": "AG_004", "Correlation ID": "CORR_ABC103", "URL Referral": "https://www.americanexpress.com/tech/news", "ContentSquare URL Link": "https://contentsquare.com/session/bcd890" }] },
        { "Row ID": 'b5', "Week": '2024-01-22', "Path": '/products', "Journey Type": "Upgraded", "Team": "US", "Business Unit": "PL", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [{ "Visitor Id": "V00205", "Visitor No": 1, "Visitor Page No": [3, 7], "MCV ID": "MCVID_EFG123", "Agent ID": "AG_005", "Correlation ID": "CORR_ABC104", "URL Referral": "https://www.americanexpress.com/promo/deals", "ContentSquare URL Link": "https://contentsquare.com/session/efg123" }] },
        { "Row ID": 'b6', "Week": '2024-01-22', "Path": '/products', "Journey Type": "Basic", "Team": "India", "Business Unit": "Consumer", "Device": "PC", "Completed": "no", "Hard Anomaly": "true", "granular_details": [{ "Visitor Id": "V00206", "Visitor No": 1, "Visitor Page No": [2, 6], "MCV ID": "MCVID_HIJ456", "Agent ID": "AG_006", "Correlation ID": "CORR_ABC105", "URL Referral": "https://www.americanexpress.com/ad/banner", "ContentSquare URL Link": "https://contentsquare.com/session/hij456" }] },
        { "Row ID": 'c1', "Week": '2024-01-29', "Path": '/checkout', "Journey Type": "Basic", "Team": "US", "Business Unit": "Consumer", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [{ "Visitor Id": "8912345678291_218899923045678H", "Visitor No": 1, "Visitor Page No": [4, 9], "MCV ID": "MCVID_KLM789", "Agent ID": "AG_001", "Correlation ID": "CORR_XYZ901", "URL Referral": "https://www.americanexpress.com/search/checkout", "ContentSquare URL Link": "https://contentsquare.com/session/klm789" }] },
        { "Row ID": 'c2', "Week": '2024-01-29', "Path": '/checkout', "Journey Type": "Upgraded", "Team": "India", "Business Unit": "PL", "Device": "PC", "Completed": "no", "Hard Anomaly": "true", "granular_details": [{ "Visitor Id": "V00302", "Visitor No": 1, "Visitor Page No": [1, 5], "MCV ID": "MCVID_NOP012", "Agent ID": "AG_002", "Correlation ID": "CORR_ABC201", "URL Referral": "https://www.americanexpress.com/posts/payment", "ContentSquare URL Link": "https://contentsquare.com/session/nop012" }] },
        { "Row ID": 'c3', "Week": '2024-01-29', "Path": '/checkout', "Journey Type": "Basic", "Team": "US", "Business Unit": "Consumer", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [{ "Visitor Id": "V00303", "Visitor No": 1, "Visitor Page No": [2, 7], "MCV ID": "MCVID_QRS345", "Agent ID": "AG_003", "Correlation ID": "CORR_ABC202", "URL Referral": "https://www.americanexpress.com/shop/cart", "ContentSquare URL Link": "https://contentsquare.com/session/qrs345" }] },
        { "Row ID": 'c4', "Week": '2024-01-29', "Path": '/checkout', "Journey Type": null, "Team": "India", "Business Unit": "sbs", "Device": "desktop", "Completed": "no", "Hard Anomaly": "true", "granular_details": [{ "Visitor Id": "V00304", "Visitor No": 1, "Visitor Page No": [6, 11], "MCV ID": "MCVID_TUV678", "Agent ID": "AG_004", "Correlation ID": "CORR_ABC203", "URL Referral": "https://www.americanexpress.com/payment/secure", "ContentSquare URL Link": "https://contentsquare.com/session/tuv678" }] },
        { "Row ID": 'c5', "Week": '2024-01-29', "Path": '/checkout', "Journey Type": "Upgraded", "Team": "US", "Business Unit": "PL", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [{ "Visitor Id": "V00305", "Visitor No": 1, "Visitor Page No": [3, 8], "MCV ID": "MCVID_WXY901", "Agent ID": "AG_005", "Correlation ID": "CORR_ABC204", "URL Referral": "https://www.americanexpress.com/offer/sale", "ContentSquare URL Link": "https://contentsquare.com/session/wxy901" }] },
        { "Row ID": 'c6', "Week": '2024-01-29', "Path": '/checkout', "Journey Type": "Basic", "Team": "India", "Business Unit": "Consumer", "Device": "PC", "Completed": "no", "Hard Anomaly": "true", "granular_details": [{ "Visitor Id": "V00306", "Visitor No": 1, "Visitor Page No": [1, 4], "MCV ID": "MCVID_ZAB234", "Agent ID": "AG_006", "Correlation ID": "CORR_ABC205", "URL Referral": "https://www.americanexpress.com/checkout/final", "ContentSquare URL Link": "https://contentsquare.com/session/zab234" }] },
        { "Row ID": 'd1', "Week": '2024-01-15', "Path": '/global-offers', "Journey Type": "Basic", "Team": "US", "Business Unit": "Consumer", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [{ "Visitor Id": "9988776655443_112233445566778A", "Visitor No": 1, "Visitor Page No": [1, 2], "MCV ID": "MCVID_GLO441", "Agent ID": "AG_010", "Correlation ID": "CORR_GLO401", "URL Referral": "https://www.americanexpress.com/global", "ContentSquare URL Link": "https://contentsquare.com/session/glo441" }] },
        { "Row ID": 'd2', "Week": '2024-01-22', "Path": '/global-offers', "Journey Type": "Upgraded", "Team": "India", "Business Unit": "PL", "Device": "desktop", "Completed": "no", "Hard Anomaly": "true", "granular_details": [{ "Visitor Id": "9988776655443_223344556677889B", "Visitor No": 1, "Visitor Page No": [3, 5], "MCV ID": "MCVID_GLO442", "Agent ID": "AG_011", "Correlation ID": "CORR_GLO402", "URL Referral": "https://www.americanexpress.com/global", "ContentSquare URL Link": "https://contentsquare.com/session/glo442" }] },
        { "Row ID": 'd3', "Week": '2024-01-29', "Path": '/global-offers', "Journey Type": "Basic", "Team": "US", "Business Unit": "sbs", "Device": "mobile", "Completed": "yes", "Hard Anomaly": "false", "granular_details": [{ "Visitor Id": "9988776655443_334455667788990C", "Visitor No": 1, "Visitor Page No": [4, 8], "MCV ID": "MCVID_GLO443", "Agent ID": "AG_012", "Correlation ID": "CORR_GLO403", "URL Referral": "https://www.americanexpress.com/global", "ContentSquare URL Link": "https://contentsquare.com/session/glo443" }] }
    ];
    const MOCK_SESSION_DATA = {};

    // ==================== MAIN APPLICATION ====================
    class AnomalyGridApp {
        constructor() {
            this.appData = SAMPLE_DATA;
            this.investigationLog = [];
            this.currentDetailRecords = [];
            this.selectedRowForInteraction = null;
            this.allWeeks = [];
            this.gridState = { filters: {}, sortColumn: 'Row ID', sortDirection: 'asc', currentPage: 1 };
            this.chartInstances = {};
            this.logCache = new MemoizationCache(500);
            this.debouncedUpdateGridView = PerformanceOptimizer.debounce(() => this._updateGridViewImmediate(), CONFIG.DEBOUNCE_DELAY);
            this.init();
        }
        
        init() {
            try {
                this.investigationLog = []; 
                this.allWeeks = [...new Set(this.appData.map(d => d["Week"]))].sort();
                this.currentDetailRecords = this.appData;
                this.renderDetailsView();
                this.updateLogView();
                this.setupEventListeners();
                UIHelpers.showToast('Application loaded successfully', 'success');
            } catch (error) { console.error('Initialization failed:', error); UIHelpers.showToast('Failed to initialize application', 'error'); }
        }
        
        setupEventListeners() {
            document.addEventListener('click', e => {
                const button = e.target.closest('.multi-select-button');
                if (button) {
                    e.stopPropagation();
                    const menu = button.nextElementSibling;
                    const isOpen = menu.classList.contains('show');
                    document.querySelectorAll('.multi-select-menu').forEach(m => m.classList.remove('show'));
                    if (!isOpen) menu.classList.add('show');
                } else if (!e.target.closest('.multi-select-dropdown')) {
                    document.querySelectorAll('.multi-select-menu').forEach(menu => menu.classList.remove('show'));
                }
            });
            
            const detailsContainer = document.getElementById('details-container');
            let gridFilterTimeout;
            detailsContainer.addEventListener('change', e => {
                if (e.target.classList.contains('multi-select-checkbox')) {
                    clearTimeout(gridFilterTimeout);
                    this.handleFilterChangeImmediate(e.target);
                    gridFilterTimeout = setTimeout(() => { this.gridState.currentPage = 1; this._updateGridViewImmediate(); }, CONFIG.DEBOUNCE_DELAY);
                } else if (e.target.id === 'classification-dropdown') {
                    document.getElementById('submit-investigation-btn').disabled = !e.target.value;
                }
            });

            detailsContainer.addEventListener('input', e => {
                if (e.target.id === 'irev-value-input') {
                    ValidationUtils.clearFieldError('irev-value-input');
                } else if (e.target.id === 'resolution-date-input') {
                    ValidationUtils.clearFieldError('resolution-date-input');
                } else if (e.target.id === 'path-filter-search') {
                    const query = e.target.value.toLowerCase();
                    const options = document.querySelectorAll('.path-filter-option');
                    options.forEach(option => {
                        const label = option.querySelector('label');
                        const path = label.textContent || label.innerText;
                        if (path.toLowerCase().includes(query)) {
                            option.style.display = 'flex';
                        } else {
                            option.style.display = 'none';
                        }
                    });
                }
            });
            
            detailsContainer.addEventListener('click', e => {
                const target = e.target;
                if (target.classList.contains('page-link') && !target.parentElement.classList.contains('disabled')) { this.gridState.currentPage = parseInt(target.dataset.page); this.updateGridView(); } 
                else if (target.classList.contains('sortable-header')) { this.handleSort(target.dataset.column); } 
                else if (target.closest('tr')?.closest('tbody')?.id === 'details-table-body') { this.handleRowClick(target.closest('tr'), e); } 
                else if (target.id === 'submit-investigation-btn') { this.handleInvestigationSubmit(); } 
                else if (target.id === 'close-case-btn') { this.handleResolutionSubmit(); } 
            });

            document.getElementById('export-csv-btn').addEventListener('click', () => { const data = this.getExportData(); DataExporter.exportToCSV(data, `filtered_view_${new Date().toISOString().split('T')[0]}.csv`); });
            document.getElementById('export-json-btn').addEventListener('click', () => { const data = this.getExportData(); DataExporter.exportToJSON(data, `filtered_view_${new Date().toISOString().split('T')[0]}.json`); });
            document.getElementById('reset-all-filters-btn').addEventListener('click', () => this.resetAllFilters());
            
            document.getElementById('analytics-tab').addEventListener('shown.bs.tab', () => this.renderAnalytics());
            document.getElementById('granular-details-content').addEventListener('click', e => { const link = e.target.closest('.visitor-id-link'); if (link?.dataset.visitorId) this.renderSessionAnalysis(link.dataset.visitorId); });
        }
        
        getExportData() {
            return this.filterData(this.currentDetailRecords).map(rec => {
                const { granular_details, ...rest } = rec;
                const logEntry = this.findLogEntry(rec["Row ID"]);
                return { 
                    ...rest, 
                    "Anomaly Type": logEntry?.classification || "N/A", 
                    "iREV Value ($)": logEntry ? `${parseFloat(logEntry.irev_value).toFixed(2)}` : "N/A", 
                    "Team Responsible to Fix": logEntry?.team_responsible || "N/A", 
                    "Resolution Date": logEntry?.resolution_date || "N/A" 
                };
            });
        }
        
        resetAllFilters() {
            const pathSearchInput = document.getElementById('path-filter-search');
            if (pathSearchInput) pathSearchInput.value = '';
            document.querySelectorAll('.path-filter-option').forEach(option => {
                option.style.display = 'flex';
            });

            const filterOptions = CONFIG.FILTERABLE_COLUMNS.reduce((acc, col) => { acc[col] = [...new Set(this.currentDetailRecords.map(row => row[col]))].sort(); return acc; }, {});
            filterOptions["Week"] = this.allWeeks;
            filterOptions["Path"] = [...new Set(this.currentDetailRecords.map(row => row["Path"]))].sort();
            
            const allColsToReset = [...CONFIG.FILTERABLE_COLUMNS, "Week", "Path"];

            allColsToReset.forEach(col => {
                this.gridState.filters[col] = new Set(filterOptions[col].map(v => String(v)));
                const allCheckbox = document.querySelector(`.multi-select-checkbox[data-column="${col}"][value="__ALL__"]`);
                if (allCheckbox) allCheckbox.checked = true;
                document.querySelectorAll(`.multi-select-checkbox[data-column="${col}"]:not([value="__ALL__"])`).forEach(cb => { cb.checked = true; });
                this.updateMultiSelectDisplay(col);
            });

            // Reset new filters
            this.gridState.filters["__InvestigationPhase"] = new Set(["Investigation Phase Open", "Investigation Phase Closed"]);
            this.gridState.filters["__ResolutionPhase"] = new Set(["Resolution Phase Open", "Resolution Phase Closed"]);
            document.querySelectorAll('.multi-select-checkbox[data-column="__InvestigationPhase"], .multi-select-checkbox[data-column="__ResolutionPhase"]').forEach(cb => { cb.checked = true; });
            this.updateMultiSelectDisplay("__InvestigationPhase");
            this.updateMultiSelectDisplay("__ResolutionPhase");

            this.gridState.sortColumn = 'Row ID';
            this.gridState.sortDirection = 'asc';
            this.gridState.currentPage = 1;
            this.updateGridView();
            UIHelpers.showToast('All grid filters reset', 'success');
        }
        
        handleFilterChangeImmediate(checkbox) {
            const column = checkbox.dataset.column, value = checkbox.value;
            if (!this.gridState.filters[column]) this.gridState.filters[column] = new Set();
            if (value === '__ALL__') {
                const allBoxes = document.querySelectorAll(`.multi-select-checkbox[data-column="${column}"]:not([value="__ALL__"])`);
                if (checkbox.checked) { allBoxes.forEach(cb => { cb.checked = true; this.gridState.filters[column].add(cb.value); }); } 
                else { this.gridState.filters[column].clear(); allBoxes.forEach(cb => cb.checked = false); }
            } else { checkbox.checked ? this.gridState.filters[column].add(value) : this.gridState.filters[column].delete(value); }
            const totalBoxes = document.querySelectorAll(`.multi-select-checkbox[data-column="${column}"]:not([value="__ALL__"])`).length;
            const allCheckbox = document.querySelector(`.multi-select-checkbox[data-column="${column}"][value="__ALL__"]`);
            if (allCheckbox) {
                allCheckbox.checked = this.gridState.filters[column].size === totalBoxes;
            }
            this.updateMultiSelectDisplay(column);
        }
        
        renderDetailsView() {
            this.currentDetailRecords = this.appData;

            // 1. Get all unique values for filters
            const filterOptions = CONFIG.FILTERABLE_COLUMNS.reduce((acc, col) => { 
                acc[col] = [...new Set(this.currentDetailRecords.map(row => row[col]))].sort(); 
                return acc; 
            }, {});
            filterOptions["Week"] = this.allWeeks;
            filterOptions["Path"] = [...new Set(this.currentDetailRecords.map(row => row["Path"]))].sort();

            // 2. Define the filter order and labels
            const filterConfig = [
                { key: "Week", label: "Filter Week Dates" },
                ...CONFIG.FILTERABLE_COLUMNS.map(col => ({ key: col, label: col })),
                { key: "Path", label: "Search Paths" },
                { key: "__InvestigationPhase", label: "Investigation Phase" },
                { key: "__ResolutionPhase", label: "Resolution Phase" }
            ];

            // 3. Build the filter panel HTML
            let filterPanelHTML = '<div class="filter-panel mb-3 row g-3">';

            for (const config of filterConfig) {
                const column = config.key;
                const label = config.label;
                const columnId = column.replace(/[^a-zA-Z0-9]/g, '_');
                
                filterPanelHTML += `<div class="col-12 col-sm-6 col-md-4 col-lg-2"><label class="form-label fw-bold small">${escapeHtml(label)}</label>`;

                if (column === "__InvestigationPhase") {
                    filterPanelHTML += `
                    <div class="multi-select-dropdown">
                        <div class="multi-select-button" data-column="__InvestigationPhase" role="button"><span>All</span></div>
                        <div class="multi-select-menu" role="listbox">
                            <div class="multi-select-option all-option">
                                <input type="checkbox" class="multi-select-checkbox" data-column="__InvestigationPhase" value="__ALL__" id="all-investigation-phase" checked>
                                <label for="all-investigation-phase" style="cursor:pointer;flex:1;">Select All</label>
                            </div>
                            <div class="multi-select-option">
                                <input type="checkbox" class="multi-select-checkbox" data-column="__InvestigationPhase" value="Investigation Phase Open" id="investigation-open" checked>
                                <label for="investigation-open" style="cursor:pointer;flex:1;"><span class="badge bg-warning text-dark">🟡 Open</span></label>
                            </div>
                            <div class="multi-select-option">
                                <input type="checkbox" class="multi-select-checkbox" data-column="__InvestigationPhase" value="Investigation Phase Closed" id="investigation-closed" checked>
                                <label for="investigation-closed" style="cursor:pointer;flex:1;"><span class="badge bg-success">🟢 Closed</span></label>
                            </div>
                        </div>
                    </div></div>`;
                } else if (column === "__ResolutionPhase") {
                    filterPanelHTML += `
                    <div class="multi-select-dropdown">
                        <div class="multi-select-button" data-column="__ResolutionPhase" role="button"><span>All</span></div>
                        <div class="multi-select-menu" role="listbox">
                            <div class="multi-select-option all-option">
                                <input type="checkbox" class="multi-select-checkbox" data-column="__ResolutionPhase" value="__ALL__" id="all-resolution-phase" checked>
                                <label for="all-resolution-phase" style="cursor:pointer;flex:1;">Select All</label>
                            </div>
                            <div class="multi-select-option">
                                <input type="checkbox" class="multi-select-checkbox" data-column="__ResolutionPhase" value="Resolution Phase Open" id="resolution-open" checked>
                                <label for="resolution-open" style="cursor:pointer;flex:1;"><span class="badge bg-warning text-dark">🟡 Open</span></label>
                            </div>
                            <div class="multi-select-option">
                                <input type="checkbox" class="multi-select-checkbox" data-column="__ResolutionPhase" value="Resolution Phase Closed" id="resolution-closed" checked>
                                <label for="resolution-closed" style="cursor:pointer;flex:1;"><span class="badge bg-success">🟢 Closed</span></label>
                            </div>
                        </div>
                    </div></div>`;
                } else {
                    // Standard multi-select dropdown
                    const values = filterOptions[column];
                    filterPanelHTML += `<div class="multi-select-dropdown">
                        <div class="multi-select-button" data-column="${escapeHtml(column)}" role="button"><span>All</span></div>
                        <div class="multi-select-menu" role="listbox">
                            <div class="multi-select-option all-option">
                                <input type="checkbox" class="multi-select-checkbox" data-column="${escapeHtml(column)}" value="__ALL__" id="all-${columnId}" checked>
                                <label for="all-${columnId}" style="cursor:pointer;flex:1;">Select All</label>
                            </div>`;

                    // Add search box for "Path" filter
                    if (column === "Path") {
                        filterPanelHTML += `<div style="padding: 0.25rem 0.5rem; border-bottom: 1px solid #dee2e6; margin-bottom: .3rem;">
                            <input type="text" class="form-control form-control-sm" id="path-filter-search" placeholder="Type to filter paths..." autocomplete="off" onclick="event.stopPropagation();">
                        </div>`;
                    }

                    // Add scrollable container for options (especially for Path)
                    const optionsListClass = (column === "Path") ? 'path-filter-options-list' : '';
                    const optionsListStyle = (column === "Path") ? 'style="max-height: 180px; overflow-y: auto;"' : '';
                    
                    filterPanelHTML += `<div class="${optionsListClass}" ${optionsListStyle}>`;
                    
                    // Add options
                    filterPanelHTML += values.map((v, i) => {
                        const optionClass = (column === "Path") ? 'path-filter-option' : ''; // Class for filtering
                        return `<div class="multi-select-option ${optionClass}" style="display: flex;">
                            <input type="checkbox" class="multi-select-checkbox" data-column="${escapeHtml(column)}" value="${escapeHtml(String(v))}" id="${columnId}-${i}" checked>
                            <label for="${columnId}-${i}" style="cursor:pointer;flex:1;">${v === null ? '(Blank)' : escapeHtml(String(v))}</label>
                        </div>`;
                    }).join('');

                    filterPanelHTML += `</div></div></div></div>`; // Close options-list, menu, dropdown, col
                }
            }
            filterPanelHTML += '</div>'; // Close filter-panel

            // 4. Set the details container HTML
            document.getElementById('details-container').innerHTML = `${filterPanelHTML}
                <div id="grid-summary" class="text-muted small mb-2"></div><div class="table-responsive" style="max-height: 50vh; overflow-y: auto;"><table class="table table-hover table-sm"><thead id="details-table-head" class="sticky-top bg-light"></thead><tbody id="details-table-body"></tbody></table></div><nav><div id="pagination-controls" class="d-flex justify-content-center mt-3"></div></nav>
                <div id="controls-container" class="mt-4 pt-3 border-top"><div id="controls-alert-spot" class="mb-3"></div><div class="row g-3 g-md-4">
                    <div class="col-12 col-lg-6"><div class="control-section"><h5 class="mb-3">Outcome of Investigation Phase</h5><div class="mb-3"><label for="classification-dropdown" class="form-label">Choose Anomaly Type</label><select id="classification-dropdown" class="form-select" disabled><option value="">Choose a classification...</option>${CONFIG.CLASSIFICATION_CATEGORIES.map(c => `<option value="${escapeHtml(c)}">${escapeHtml(c)}</option>`).join('')}</select></div><div class="mb-3"><label for="irev-value-input" class="form-label">Anomaly iREV value (in $)<small class="irev-hint">This is optional and will default to 0 if left blank</small></label><input type="number" id="irev-value-input" class="form-control" placeholder="e.g., 450.5" min="0" step="0.00001" disabled><div id="irev-value-input-error" class="validation-error"></div></div><div class="d-flex justify-content-end"><button id="submit-investigation-btn" class="btn btn-primary" disabled>Submit Anomaly Type</button></div></div></div>
                    <div class="col-12 col-lg-6"><div class="control-section"><h5 class="mb-3">Outcome of Resolution Phase</h5><div class="mb-3"><label for="team-responsible-select" class="form-label">Team Responsible to Fix</label><select id="team-responsible-select" class="form-select" disabled><option value="">Select team email...</option>${CONFIG.RESPONSIBLE_TEAMS.map(t => `<option value="${escapeHtml(t)}">${escapeHtml(t)}</option>`).join('')}</select></div><div class="mb-3"><label for="resolution-date-input" class="form-label">Date of Resolution</label><input type="date" id="resolution-date-input" class="form-control" min="1000-01-01" max="9999-12-31" disabled><div id="resolution-date-input-error" class="validation-error"></div></div><div class="d-flex justify-content-end"><button id="close-case-btn" class="btn btn-success" disabled>Close Case</button></div></div></div>
                </div></div>`;
            
            // 5. Initialize filters state
            [...CONFIG.FILTERABLE_COLUMNS, "Week", "Path"].forEach(col => {
                this.gridState.filters[col] = new Set(filterOptions[col].map(v => String(v)));
            });
            this.gridState.filters["__InvestigationPhase"] = new Set(["Investigation Phase Open", "Investigation Phase Closed"]);
            this.gridState.filters["__ResolutionPhase"] = new Set(["Resolution Phase Open", "Resolution Phase Closed"]);

            // 6. Update the grid
            this.updateGridView();
        }
        
        updateGridView() { this.debouncedUpdateGridView(); }
        _updateGridViewImmediate() {
            let processedData = this.sortData(this.filterData(this.currentDetailRecords));
            const total = processedData.length;
            const start = total === 0 ? 0 : (this.gridState.currentPage - 1) * CONFIG.ROWS_PER_PAGE + 1;
            const end = Math.min(start + CONFIG.ROWS_PER_PAGE - 1, total);
            document.getElementById('grid-summary').textContent = total > 0 ? `Showing ${start}-${end} of ${total} records.` : 'No records match your filters.';
            this.renderTablePage(processedData);
            this.renderPaginationControls(total);
        }
        
        // ==================== MODIFIED filterData FUNCTION ====================
        filterData(data) {
            // 1. Augment data with computed phase properties
            const augmentedData = data.map(row => {
                const logEntry = this.findLogEntry(row["Row ID"]);
                
                const investigationPhase = (logEntry && logEntry.classification) 
                    ? "Investigation Phase Closed" 
                    : "Investigation Phase Open";
                
                const resolutionPhase = (logEntry && logEntry.team_responsible && logEntry.resolution_date) 
                    ? "Resolution Phase Closed" 
                    : "Resolution Phase Open";
                
                return {
                    ...row,
                    "__InvestigationPhase": investigationPhase,
                    "__ResolutionPhase": resolutionPhase
                };
            });

            // 2. Run the filter logic on the augmented data
            // "Path" is now a standard filter in this.gridState.filters
            return augmentedData.filter(row => {
                return Object.entries(this.gridState.filters).every(([col, val]) => 
                    !val || val.size === 0 ? false : val.has(String(row[col]))
                );
            });
        }
        // ==================== END OF MODIFIED FUNCTION ====================

        sortData(data) { return [...data].sort((a, b) => { const vA = a[this.gridState.sortColumn], vB = b[this.gridState.sortColumn]; if (vA < vB) return this.gridState.sortDirection === 'asc' ? -1 : 1; if (vA > vB) return this.gridState.sortDirection === 'asc' ? 1 : -1; return 0; }); }
        
        renderTablePage(data) {
            const tableHead = document.getElementById('details-table-head');
            const tableBody = document.getElementById('details-table-body');
            if (!tableHead || !tableBody) return;
            const start = (this.gridState.currentPage - 1) * CONFIG.ROWS_PER_PAGE;
            const paginatedItems = data.slice(start, start + CONFIG.ROWS_PER_PAGE);
            let headerHTML = '<tr>';
            headerHTML += CONFIG.DISPLAY_COLUMNS.map(h => {
                const arrow = this.gridState.sortColumn === h ? (this.gridState.sortDirection === 'asc' ? ' ▲' : ' ▼') : '';
                return `<th scope="col" class="${["Anomaly Type", "iREV Value ($)", "Team Responsible to Fix", "Resolution Date"].includes(h) ? '' : 'sortable-header'}" data-column="${escapeHtml(h)}">${escapeHtml(h)}${arrow}</th>`;
            }).join('') + '</tr>';
            tableHead.innerHTML = headerHTML;
            tableBody.innerHTML = paginatedItems.map(rec => {
                const logEntry = this.findLogEntry(rec["Row ID"]);
                const rowClass = logEntry ? (logEntry.resolution_date ? 'closed-row' : 'classified-row') : '';
                let rowHTML = `<tr class="${rowClass}" style="cursor:pointer" data-row-id="${escapeHtml(rec["Row ID"])}">`;
                rowHTML += CONFIG.DISPLAY_COLUMNS.map(h => {
                    let cellValue = '<em>N/A</em>';
                    if (h === 'Anomaly Type') cellValue = logEntry ? escapeHtml(logEntry.classification) : '<em>N/A</em>';
                    else if (h === 'iREV Value ($)') cellValue = logEntry ? `${parseFloat(logEntry.irev_value).toFixed(2)}` : '<em>N/A</em>';
                    else if (h === 'Team Responsible to Fix') cellValue = logEntry?.team_responsible ? escapeHtml(logEntry.team_responsible) : '<em>N/A</em>';
                    else if (h === 'Resolution Date') cellValue = logEntry?.resolution_date ? escapeHtml(logEntry.resolution_date) : '<em>N/A</em>';
                    else cellValue = rec[h] === null ? '<em>(Blank)</em>' : escapeHtml(String(rec[h]));
                    return `<td>${cellValue}</td>`;
                }).join('') + '</tr>';
                return rowHTML;
            }).join('');
        }
        
        getCurrentPageRecords() { return this.sortData(this.filterData(this.currentDetailRecords)).slice((this.gridState.currentPage - 1) * CONFIG.ROWS_PER_PAGE, this.gridState.currentPage * CONFIG.ROWS_PER_PAGE); }
        renderPaginationControls(totalRecords) {
            const container = document.getElementById('pagination-controls');
            const totalPages = Math.ceil(totalRecords / CONFIG.ROWS_PER_PAGE);
            if (totalPages <= 1) { container.innerHTML = ''; return; }
            let html = '<ul class="pagination pagination-sm flex-wrap">';
            html += `<li class="page-item ${this.gridState.currentPage === 1 ? 'disabled' : ''}"><a class="page-link" data-page="${this.gridState.currentPage - 1}">Previous</a></li>`;
            for (let i = 1; i <= totalPages; i++) html += `<li class="page-item ${i === this.gridState.currentPage ? 'active' : ''}"><a class="page-link" data-page="${i}">${i}</a></li>`;
            html += `<li class="page-item ${this.gridState.currentPage === totalPages ? 'disabled' : ''}"><a class="page-link" data-page="${this.gridState.currentPage + 1}">Next</a></li>`;
            container.innerHTML = html + '</ul>';
        }
        
        updateMultiSelectDisplay(column) {
            const span = document.querySelector(`.multi-select-button[data-column="${column}"] span`);
            if (!span) return;
            const count = this.gridState.filters[column]?.size || 0;
            const total = document.querySelectorAll(`.multi-select-checkbox[data-column="${column}"]:not([value="__ALL__"])`).length;
            if (count === 0) { span.textContent = 'None'; span.className = 'text-muted'; }
            else if (count === total) { span.textContent = 'All'; span.className = ''; }
            else { span.textContent = `${count} selected`; span.className = 'selected-count'; }
        }
        
        handleSort(column) {
            if (this.gridState.sortColumn === column) this.gridState.sortDirection = this.gridState.sortDirection === 'asc' ? 'desc' : 'asc';
            else { this.gridState.sortColumn = column; this.gridState.sortDirection = 'asc'; }
            this.gridState.currentPage = 1;
            this.updateGridView();
        }
        
        handleRowClick(row, e) {
            if (!row) return;
            const rowId = row.dataset.rowId;
            this.selectedRowForInteraction = this.currentDetailRecords.find(d => d["Row ID"] === rowId);
            document.querySelectorAll('#details-table-body tr').forEach(r => r.classList.remove('table-primary'));
            row.classList.add('table-primary');
            this.updateControlPanelState();
            
            //Always load granular details regardless of investigation status
            //if (!this.isRowInvestigated(rowId)) this.loadGranularDetails(this.selectedRowForInteraction);
            this.loadGranularDetails(this.selectedRowForInteraction);
        }
        
        updateControlPanelState() {
            if (!this.selectedRowForInteraction) return;
            const logEntry = this.findLogEntry(this.selectedRowForInteraction["Row ID"]);
            const alertSpot = document.getElementById('controls-alert-spot');
            alertSpot.innerHTML = '';
            
            const classificationDropdown = document.getElementById('classification-dropdown'), irevInput = document.getElementById('irev-value-input'), submitBtn = document.getElementById('submit-investigation-btn');
            const teamSelect = document.getElementById('team-responsible-select'), dateInput = document.getElementById('resolution-date-input'), closeBtn = document.getElementById('close-case-btn');

            [classificationDropdown, teamSelect, irevInput, dateInput].forEach(el => el.value = "");
            ValidationUtils.clearFieldError('irev-value-input');
            ValidationUtils.clearFieldError('resolution-date-input');
            
            if (logEntry) {
                classificationDropdown.value = logEntry.classification;
                irevInput.value = parseFloat(logEntry.irev_value).toFixed(4);
                [classificationDropdown, irevInput, submitBtn].forEach(el => el.disabled = true);

                if (logEntry.resolution_date) {
                    alertSpot.innerHTML = `<div class="alert alert-primary">The investigation for this Path has been successfully closed.</div>`;
                    teamSelect.value = logEntry.team_responsible;
                    dateInput.value = logEntry.resolution_date;
                    [teamSelect, dateInput, closeBtn].forEach(el => el.disabled = true);
                } else {
                    alertSpot.innerHTML = `<div class="alert alert-info">This Path has been classified as <strong>${escapeHtml(logEntry.classification)}</strong>. You may now proceed with the Resolution Phase.</div>`;
                    [teamSelect, dateInput, closeBtn].forEach(el => el.disabled = false);
                    closeBtn.removeAttribute('data-bs-toggle');
                    closeBtn.removeAttribute('title');
                }
            } else {
                [classificationDropdown, irevInput].forEach(el => el.disabled = false);
                submitBtn.disabled = true; 
                [teamSelect, dateInput, closeBtn].forEach(el => el.disabled = true);
                closeBtn.setAttribute('data-bs-toggle', 'tooltip');
                closeBtn.setAttribute('title', '⚠️ Please classify the Anomaly Type first, if not done already');
                new bootstrap.Tooltip(closeBtn);
            }
        }

        findLogEntry(rowId) { if (this.logCache.has(rowId)) return this.logCache.get(rowId); const entry = this.investigationLog.find(a => a.row_id === rowId); this.logCache.set(rowId, entry || null); return entry; }
        isRowInvestigated(rowId) { return !!this.findLogEntry(rowId); }
        
        async handleInvestigationSubmit() {
            try {
                if (!this.selectedRowForInteraction) throw new Error("Please select a row first");
                const classification = document.getElementById('classification-dropdown').value;
                if (!classification) throw new Error("Please select a classification");
                if (this.isRowInvestigated(this.selectedRowForInteraction["Row ID"])) throw new Error(`This row is already classified.`);
                
                const irevInput = document.getElementById('irev-value-input');
                const validation = ValidationUtils.validateIrevValue(irevInput.value);
                
                if (!validation.valid) {
                    ValidationUtils.showFieldError('irev-value-input', validation.error);
                    throw new Error(validation.error);
                }
                
                const confirmed = await UIHelpers.showConfirmationDialog({ 
                    title: 'Are you sure you want to classify this anomaly?', 
                    path: this.selectedRowForInteraction.Path, 
                    classification, 
                    irev: validation.value, 
                    weeks: [this.selectedRowForInteraction.Week], 
                    affectedRows: 1, 
                    confirmText: 'Yes, Submit Anomaly Type' 
                });
                if (confirmed) await this.saveInvestigation(this.selectedRowForInteraction, classification, validation.value);
            } catch (error) { UIHelpers.showToast(error.message, 'error'); }
        }

        async handleResolutionSubmit() {
            try {
                if (!this.selectedRowForInteraction) throw new Error("Please select a row first");
                const logEntry = this.findLogEntry(this.selectedRowForInteraction["Row ID"]);
                if (!logEntry) throw new Error("This row must be investigated before it can be closed.");

                const team = document.getElementById('team-responsible-select').value;
                const dateInput = document.getElementById('resolution-date-input');
                const date = dateInput.value;
                
                if (!team) throw new Error("Please select the team responsible.");
                if (!date) throw new Error("Please enter the date of resolution.");

                const dateValidation = ValidationUtils.validateResolutionDate(date);
                if (!dateValidation.valid) {
                    ValidationUtils.showFieldError('resolution-date-input', dateValidation.error);
                    throw new Error(dateValidation.error);
                }

                const confirmed = await UIHelpers.showConfirmationDialog({ 
                    title: 'Are you sure you want to close this case?', 
                    path: this.selectedRowForInteraction.Path, 
                    classification: logEntry.classification, 
                    weeks: [this.selectedRowForInteraction.Week], 
                    affectedRows: 1, 
                    teamResponsible: team, 
                    resolutionDate: date, 
                    confirmText: 'Yes, Close Case' 
                });
                if (confirmed) await this.saveResolution(logEntry, team, date);
            } catch (error) { UIHelpers.showToast(error.message, 'error'); }
        }
        
        async saveInvestigation(row, classification, irevValue) {
            UIHelpers.showLoading('Saving classification...');
            try {
                const logEntry = { 
                    row_id: row["Row ID"], 
                    week: row.Week, 
                    path: row.Path, 
                    classification, 
                    irev_value: irevValue, 
                    investigation_timestamp: formatTimestamp(new Date()), 
                    team_responsible: null, 
                    resolution_date: null, 
                    resolution_timestamp: null 
                };
                this.investigationLog.push(logEntry);
                this.logCache.set(row["Row ID"], logEntry);
                this.updateLogView();
                this.updateGridView();
                this.updateControlPanelState();
                UIHelpers.showToast('Classification saved successfully', 'success');
            } finally { UIHelpers.hideLoading(); }
        }

        async saveResolution(logEntry, team, date) {
            UIHelpers.showLoading('Closing case...');
            try {
                logEntry.team_responsible = team;
                logEntry.resolution_date = date;
                logEntry.resolution_timestamp = formatTimestamp(new Date());
                this.logCache.set(logEntry.row_id, logEntry);
                this.updateLogView();
                this.updateGridView();
                this.updateControlPanelState();
                UIHelpers.showToast('Case closed successfully', 'success');
            } finally { UIHelpers.hideLoading(); }
        }
        
        loadGranularDetails(rowData) {
            if (!rowData) return;
            const headers = ["Row ID", "Week", "Path", "Journey Type", "Team", "Business Unit", "Device", "Completed", "Hard Anomaly", "Visitor Id", "Visitor No", "Visitor Page No", "MCV ID", "Agent ID", "Correlation ID", "URL Referral", "ContentSquare URL Link"];
            
            // 1. Generate all flat data
            let flatData = [];
            if (rowData.granular_details) {
                rowData.granular_details.forEach(detail => {
                    if (detail["Visitor Page No"] && Array.isArray(detail["Visitor Page No"])) {
                        detail["Visitor Page No"].forEach(pgNo => {
                            const newRowData = { ...rowData };
                            delete newRowData.granular_details;
                            flatData.push({ ...newRowData, ...detail, "Visitor Page No": pgNo });
                        });
                    }
                });
            }
            
            const totalCases = flatData.length;
            let sampledData = [];
            let sampleSize = 20;

            // 2. Create sampled data
            if (totalCases <= sampleSize) {
                sampledData = flatData;
                sampleSize = totalCases;
            } else {
                // Fisher-Yates shuffle
                let shuffled = [...flatData];
                for (let i = shuffled.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
                }
                sampledData = shuffled.slice(0, sampleSize);
            }

            // 3. Generate HTML with new header
            let html = `
                <div class="d-flex justify-content-between flex-wrap gap-2 mb-3">
                    <button class="btn btn-secondary btn-sm" onclick="app.goBackToExplorer()">← Back to Anomaly Inventory</button>
                    <button class="btn btn-primary btn-sm" onclick="app.goBackToExplorer()">🏠 Home Page</button>
                </div>
                <h4 class="mb-3">
                    Illustrative Cases for Row ID: <span class="text-primary fw-bold">${escapeHtml(rowData["Row ID"])}</span>
                </h4>
                <p class="text-muted" style="font-size: 0.9rem;">
                    Total Illustrative Cases for Row ID ${escapeHtml(rowData["Row ID"])} is <strong>${totalCases.toLocaleString()}</strong>. 
                    Showing <strong>${sampleSize}</strong> sample rows only.
                </p>
                <div class="info-box">💡 Click any <strong>Visitor ID</strong> to view session analysis.</div>
            `;

            // 4. Generate table from sampled data
            if (sampledData.length > 0) {
                html += `<div class="table-responsive"><table class="table table-bordered table-sm table-hover"><thead><tr>${headers.map(h => `<th>${escapeHtml(h)}</th>`).join('')}</tr></thead><tbody>${sampledData.map(r => `<tr><td>${escapeHtml(r["Row ID"])}</td><td>${escapeHtml(r.Week)}</td><td>${escapeHtml(r.Path)}</td><td>${r["Journey Type"] ? escapeHtml(r["Journey Type"]) : '<em>-</em>'}</td><td>${escapeHtml(r.Team)}</td><td>${escapeHtml(r["Business Unit"])}</td><td>${r.Device ? escapeHtml(r.Device) : '<em>-</em>'}</td><td>${escapeHtml(r.Completed)}</td><td>${escapeHtml(r["Hard Anomaly"])}</td><td><a class="visitor-id-link" data-visitor-id="${escapeHtml(r["Visitor Id"])}">${escapeHtml(r["Visitor Id"])}</a></td><td>${escapeHtml(String(r["Visitor No"]))}</td><td>${escapeHtml(String(r["Visitor Page No"]))}</td><td>${escapeHtml(r["MCV ID"])}</td><td>${escapeHtml(r["Agent ID"])}</td><td>${escapeHtml(r["Correlation ID"])}</td><td><a href="${escapeHtml(r["URL Referral"])}" target="_blank">${escapeHtml((r["URL Referral"] || '').substring(0, 40))}...</a></td><td><a href="${escapeHtml(r["ContentSquare URL Link"])}" target="_blank">View Session</a></td></tr>`).join('')}</tbody></table></div>`;
            } else { html += '<div class="alert alert-warning mt-3">No granular visitor details available.</div>'; }
            
            document.getElementById('granular-details-content').innerHTML = html;
            new bootstrap.Tab(document.getElementById('granular-tab')).show();
        }
        
        goBackToExplorer() { new bootstrap.Tab(document.getElementById('explorer-tab')).show(); }
        
        updateLogView() {
            const head = document.getElementById('log-table-head'), body = document.getElementById('log-table-body');
            if (!head || !body) return;
            head.innerHTML = `<tr>${CONFIG.LOG_DISPLAY_COLUMNS.map(h => `<th scope="col">${escapeHtml(h)}</th>`).join('')}</tr>`;
            const sortedLog = [...this.investigationLog].sort((a, b) => new Date(b.investigation_timestamp) - new Date(a.investigation_timestamp));
            body.innerHTML = sortedLog.map(log => `<tr><td>${escapeHtml(log.row_id)}</td><td>${escapeHtml(log.week)}</td><td><span class="path-badge">${escapeHtml(log.path)}</span></td><td>${escapeHtml(log.classification)}</td><td>${parseFloat(log.irev_value).toFixed(2)}</td><td>${escapeHtml(log.investigation_timestamp)}</td><td>${escapeHtml(log.team_responsible || '-')}</td><td>${escapeHtml(log.resolution_date || '-')}</td><td>${escapeHtml(log.resolution_timestamp || '-')}</td></tr>`).join('');
        }
        
        renderAnalytics() {
            const container = document.getElementById('analytics-content');
            if (this.investigationLog.length === 0) { container.innerHTML = `<div class="text-center p-5"><p class="text-muted">No classification data available.</p></div>`; return; }
            
            const totalIrev = this.investigationLog.reduce((s, l) => s + parseFloat(l.irev_value), 0);
            
            container.innerHTML = `<div class="row g-3 g-md-4">
                <div class="col-12 col-lg-6"><div class="card h-100"><div class="card-body"><h5 class="card-title">Classification Counts</h5><div class="chart-container"><canvas id="pie-chart"></canvas></div></div></div></div>
                <div class="col-12 col-lg-6"><div class="card h-100"><div class="card-body"><h5 class="card-title">Classifications by Path</h5><div class="chart-container"><canvas id="bar-chart-path"></canvas></div></div></div></div>
                <div class="col-12 col-lg-6"><div class="card h-100"><div class="card-body"><h5 class="card-title">Total iREV by Team (US/India)</h5><div class="chart-container"><canvas id="bar-chart-irev-team"></canvas></div></div></div></div>
                <div class="col-12 col-lg-6"><div class="card h-100"><div class="card-body"><h5 class="card-title">Total iREV by Responsible Team</h5><div class="chart-container"><canvas id="bar-chart-irev-responsible"></canvas></div></div></div></div>
            </div>
            <div class="card mt-3 mt-md-4"><div class="card-body"><h5 class="card-title">Summary Statistics</h5><div class="row text-center g-3">
                <div class="col-6 col-md"><h3 class="text-primary">${this.investigationLog.length}</h3><p class="text-muted small">Total Classified</p></div>
                <div class="col-6 col-md"><h3 class="text-primary">${this.investigationLog.filter(l => l.resolution_date).length}</h3><p class="text-muted small">Total Closed</p></div>
                <div class="col-6 col-md"><h3 class="text-success">${new Set(this.investigationLog.map(l => l.path)).size}</h3><p class="text-muted small">Unique Paths</p></div>
                <div class="col-6 col-md"><h3 class="text-warning">${new Set(this.investigationLog.map(l => l.classification)).size}</h3><p class="text-muted small">Classifications Used</p></div>
                <div class="col-12 col-md"><h3 class="text-info">${totalIrev.toFixed(2)}</h3><p class="text-muted small">Total iREV</p></div>
            </div></div></div>`;
            this.renderCharts();
        }
        
        renderCharts() {
            Object.values(this.chartInstances).forEach(chart => chart.destroy());
            this.chartInstances = {};
            const generateChart = (ctxId, type, data, options) => { const ctx = document.getElementById(ctxId); if (ctx) this.chartInstances[ctxId] = new Chart(ctx, { type, data, options }); };
            const colors = ['#0d6efd', '#198754', '#ffc107', '#dc3545', '#6f42c1', '#fd7e14', '#20c997', '#0dcaf0', '#d63384', '#6610f2'];
            const chartOptions = { responsive: true, maintainAspectRatio: false };

            const classificationCounts = this.investigationLog.reduce((a, l) => { a[l.classification] = (a[l.classification] || 0) + 1; return a; }, {});
            generateChart('pie-chart', 'pie', { labels: Object.keys(classificationCounts), datasets: [{ data: Object.values(classificationCounts), backgroundColor: colors }] }, { ...chartOptions, plugins: { legend: { position: 'bottom' } } });

            const pathCounts = this.investigationLog.reduce((a, l) => { a[l.path] = (a[l.path] || 0) + 1; return a; }, {});
            generateChart('bar-chart-path', 'bar', { labels: Object.keys(pathCounts), datasets: [{ label: 'Classifications', data: Object.values(pathCounts), backgroundColor: '#0dcaf0' }] }, { ...chartOptions, plugins: { legend: { display: false } }, scales: { y: { beginAtZero: true, ticks: { stepSize: 1 } } } });

            const irevByTeam = this.investigationLog.reduce((a, l) => { const r = this.appData.find(d => d["Row ID"] === l.row_id); if (r) { a[r.Team] = (a[r.Team] || 0) + parseFloat(l.irev_value); } return a; }, {});
            generateChart('bar-chart-irev-team', 'bar', { labels: Object.keys(irevByTeam), datasets: [{ label: 'Total iREV ($)', data: Object.values(irevByTeam), backgroundColor: '#198754' }] }, { ...chartOptions, plugins: { legend: { display: false } }, scales: { y: { beginAtZero: true } } });

            const irevByResponsible = this.investigationLog.filter(l => l.team_responsible).reduce((a, l) => { a[l.team_responsible] = (a[l.team_responsible] || 0) + parseFloat(l.irev_value); return a; }, {});
            generateChart('bar-chart-irev-responsible', 'bar', { labels: Object.keys(irevByResponsible), datasets: [{ label: 'Total iREV ($)', data: Object.values(irevByResponsible), backgroundColor: '#6f42c1' }] }, { ...chartOptions, indexAxis: 'y', plugins: { legend: { display: false } }, scales: { x: { beginAtZero: true } } });
        }
        
        generateFullSessionData() {
            const evar74 = ["US:PartnerProspect:SBS:Landing:BillingInfo", "US:AcctMgmt:Login:Success", "US:CardApply:SubmitDecision"], evar30 = ["Enrollment:Start", "Enrollment:Data", "Login:Attempt", "Page:View"];
            [...new Set(SAMPLE_DATA.flatMap(d => d.granular_details.map(gd => gd["Visitor Id"])))].forEach(visitorId => {
                MOCK_SESSION_DATA[visitorId] = [];
                for (let v = 1; v <= Math.floor(Math.random() * 3) + 1; v++) {
                    let p = Math.floor(Math.random() * 5) + 1;
                    for (let i = 0; i < Math.floor(Math.random() * 10) + 2; i++) {
                        MOCK_SESSION_DATA[visitorId].push({ "visitor id": visitorId, "visitor no": v, "page no": p, "post_evar30": evar30[Math.floor(Math.random() * evar30.length)], "post_evar50 (URL)": `https://example.com/stp-id=${Math.random().toString(36).substring(2, 10)}`, "post_evar74": evar74[Math.floor(Math.random() * evar74.length)], "post_prop22": i % 4 === 0 ? "ChangeInError" : "" });
                        p += Math.floor(Math.random() * 5) + 2;
                    }
                }
            });
        }
        
        renderSessionAnalysis(visitorId) {
            const container = document.getElementById('session-analysis-content');
            const data = MOCK_SESSION_DATA[visitorId] || [];
            const pageViews = data.length;
            const visits = new Set(data.map(e => e["visitor no"])).size;
            
            let html = `
                <div class="d-flex justify-content-between flex-wrap gap-2 mb-3">
                    <button class="btn btn-secondary btn-sm" onclick="app.goBackToIllustrativeCases()">← Back to Illustrative Cases</button>
                    <button class="btn btn-primary btn-sm" onclick="app.goBackToExplorer()">🏠 Home Page</button>
                </div>
                <h3>Session Analysis: <span class="text-primary fw-bold">${escapeHtml(visitorId)}</span></h3>
                <p class="text-muted">Showing ${pageViews} page views across ${visits} visit(s).</p>
            `;
            if (data.length > 0) {
                const headers = Object.keys(data[0]);
                html += `<div class="table-responsive"><table class="table table-sm table-striped"><thead><tr>${headers.map(h => `<th>${escapeHtml(h)}</th>`).join('')}</tr></thead><tbody>${data.map(r => `<tr>${headers.map(h => `<td>${escapeHtml(r[h])}</td>`).join('')}</tr>`).join('')}</tbody></table></div>`;
            } else { html += `<div class="alert alert-warning">No session data found.</div>`; }
            container.innerHTML = html;
            new bootstrap.Tab(document.getElementById('session-analysis-tab-btn')).show();
        }
        
        goBackToIllustrativeCases() { new bootstrap.Tab(document.getElementById('granular-tab')).show(); }
    }

    // ==================== INITIALIZATION ====================
    let app;
    document.addEventListener('DOMContentLoaded', function() {
        try {
            app = new AnomalyGridApp();
            window.app = app;
            setTimeout(() => app.generateFullSessionData(), 0);
        } catch (error) {
            console.error('Failed to start application:', error);
            alert('Failed to start application. Please refresh the page.');
        }
    });
    </script>
</body>
</html>
