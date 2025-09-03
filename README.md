# FPSOW92
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SOW Builder - Live Data Integration</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary: #2563eb;
            --primary-dark: #1e40af;
            --secondary: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --gray-100: #f3f4f6;
            --gray-200: #e5e7eb;
            --gray-300: #d1d5db;
            --gray-400: #9ca3af;
            --gray-500: #6b7280;
            --gray-600: #4b5563;
            --gray-700: #374151;
            --gray-800: #1f2937;
            --gray-900: #111827;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        .header {
            background: white;
            border-radius: 12px;
            padding: 24px;
            margin-bottom: 24px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .header h1 {
            color: var(--gray-900);
            font-size: 28px;
            margin-bottom: 8px;
        }

        .header p {
            color: var(--gray-600);
            font-size: 14px;
        }

        .data-upload-section {
            background: white;
            border-radius: 12px;
            padding: 24px;
            margin-bottom: 24px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .upload-area {
            border: 2px dashed var(--gray-300);
            border-radius: 8px;
            padding: 32px;
            text-align: center;
            transition: all 0.3s;
            cursor: pointer;
            position: relative;
        }

        .upload-area:hover {
            border-color: var(--primary);
            background: rgba(37, 99, 235, 0.05);
        }

        .upload-area.dragover {
            border-color: var(--primary);
            background: rgba(37, 99, 235, 0.1);
        }

        .upload-area.has-data {
            border-color: var(--secondary);
            background: rgba(16, 185, 129, 0.05);
        }

        .upload-icon {
            font-size: 48px;
            margin-bottom: 16px;
        }

        .upload-text {
            color: var(--gray-700);
            font-size: 16px;
            margin-bottom: 8px;
        }

        .upload-subtext {
            color: var(--gray-500);
            font-size: 14px;
        }

        input[type="file"] {
            position: absolute;
            opacity: 0;
            width: 100%;
            height: 100%;
            cursor: pointer;
        }

        .data-status {
            margin-top: 16px;
            padding: 12px;
            border-radius: 8px;
            display: none;
        }

        .data-status.success {
            background: rgba(16, 185, 129, 0.1);
            border: 1px solid var(--secondary);
            display: block;
        }

        .data-status.error {
            background: rgba(239, 68, 68, 0.1);
            border: 1px solid var(--danger);
            display: block;
        }

        .data-stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 12px;
            margin-top: 16px;
        }

        .stat-card {
            background: var(--gray-50);
            padding: 12px;
            border-radius: 6px;
        }

        .stat-label {
            font-size: 12px;
            color: var(--gray-600);
            margin-bottom: 4px;
        }

        .stat-value {
            font-size: 20px;
            font-weight: 600;
            color: var(--gray-900);
        }

        .main-content {
            display: grid;
            grid-template-columns: 1fr 400px;
            gap: 24px;
        }

        .form-section, .sidebar {
            background: white;
            border-radius: 12px;
            padding: 24px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .step-indicator {
            display: flex;
            justify-content: space-between;
            margin-bottom: 32px;
            padding-bottom: 24px;
            border-bottom: 1px solid var(--gray-200);
        }

        .step {
            flex: 1;
            text-align: center;
            position: relative;
        }

        .step::after {
            content: '';
            position: absolute;
            top: 15px;
            right: -50%;
            width: 100%;
            height: 2px;
            background: var(--gray-300);
            z-index: 1;
        }

        .step:last-child::after {
            display: none;
        }

        .step-number {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: var(--gray-300);
            color: white;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 14px;
            position: relative;
            z-index: 2;
            margin-bottom: 8px;
        }

        .step.active .step-number {
            background: var(--primary);
        }

        .step.completed .step-number {
            background: var(--secondary);
        }

        .step-label {
            font-size: 12px;
            color: var(--gray-600);
            font-weight: 500;
        }

        .form-group {
            margin-bottom: 24px;
        }

        label {
            display: block;
            font-size: 14px;
            font-weight: 600;
            color: var(--gray-700);
            margin-bottom: 8px;
        }

        select, input, textarea {
            width: 100%;
            padding: 10px 12px;
            border: 1px solid var(--gray-300);
            border-radius: 8px;
            font-size: 14px;
            transition: all 0.2s;
        }

        select:focus, input:focus, textarea:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
        }

        textarea {
            resize: vertical;
            min-height: 100px;
        }

        .archetype-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin-top: 12px;
        }

        .archetype-card {
            padding: 16px;
            border: 2px solid var(--gray-200);
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
            position: relative;
        }

        .archetype-card:hover {
            border-color: var(--primary);
            box-shadow: 0 4px 12px rgba(37, 99, 235, 0.15);
        }

        .archetype-card.selected {
            border-color: var(--primary);
            background: rgba(37, 99, 235, 0.05);
        }

        .archetype-card.selected::after {
            content: '✓';
            position: absolute;
            top: 8px;
            right: 8px;
            width: 20px;
            height: 20px;
            background: var(--primary);
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
        }

        .archetype-title {
            font-weight: 600;
            color: var(--gray-900);
            margin-bottom: 4px;
        }

        .archetype-desc {
            font-size: 12px;
            color: var(--gray-600);
            margin-bottom: 8px;
        }

        .archetype-stats {
            display: flex;
            gap: 12px;
            font-size: 11px;
            color: var(--gray-500);
        }

        .archetype-stats.live-data {
            color: var(--secondary);
            font-weight: 600;
        }

        .historical-data {
            background: var(--gray-100);
            border-radius: 8px;
            padding: 16px;
            margin-top: 16px;
        }

        .historical-data h4 {
            font-size: 14px;
            font-weight: 600;
            color: var(--gray-700);
            margin-bottom: 12px;
        }

        .data-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 13px;
        }

        .data-label {
            color: var(--gray-600);
        }

        .data-value {
            font-weight: 600;
            color: var(--gray-900);
        }

        .suggested-value {
            display: inline-block;
            background: var(--primary);
            color: white;
            padding: 2px 8px;
            border-radius: 12px;
            font-size: 11px;
            margin-left: 8px;
            cursor: pointer;
        }

        .sidebar h3 {
            font-size: 16px;
            font-weight: 600;
            color: var(--gray-900);
            margin-bottom: 16px;
        }

        .similar-projects {
            margin-top: 24px;
        }

        .project-item {
            padding: 12px;
            background: var(--gray-50);
            border-radius: 8px;
            margin-bottom: 8px;
            font-size: 13px;
            border-left: 3px solid var(--primary);
            cursor: pointer;
            transition: all 0.2s;
        }

        .project-item:hover {
            background: var(--gray-100);
            transform: translateX(2px);
        }

        .project-name {
            font-weight: 600;
            color: var(--gray-900);
            margin-bottom: 4px;
        }

        .project-details {
            color: var(--gray-600);
            font-size: 12px;
        }

        .button-group {
            display: flex;
            gap: 12px;
            margin-top: 32px;
        }

        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            background: var(--primary-dark);
        }

        .btn-secondary {
            background: white;
            color: var(--gray-700);
            border: 1px solid var(--gray-300);
        }

        .btn-secondary:hover {
            background: var(--gray-100);
        }

        .btn-success {
            background: var(--secondary);
            color: white;
        }

        .deliverable-item {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 8px;
            background: var(--gray-50);
            border-radius: 6px;
            margin-bottom: 8px;
        }

        .deliverable-item input[type="checkbox"] {
            width: auto;
        }

        .deliverable-item label {
            margin: 0;
            font-weight: normal;
            flex: 1;
        }

        .preview-section {
            background: var(--gray-50);
            border-radius: 8px;
            padding: 20px;
            margin-top: 24px;
        }

        .preview-section h3 {
            color: var(--gray-900);
            margin-bottom: 16px;
        }

        .preview-content {
            font-size: 14px;
            line-height: 1.6;
            color: var(--gray-700);
        }

        .preview-content strong {
            color: var(--gray-900);
        }

        .live-data-badge {
            display: inline-block;
            background: var(--secondary);
            color: white;
            padding: 2px 8px;
            border-radius: 4px;
            font-size: 10px;
            font-weight: 600;
            margin-left: 8px;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% {
                opacity: 1;
            }
            50% {
                opacity: 0.7;
            }
        }

        .confidence-indicator {
            display: inline-flex;
            align-items: center;
            gap: 4px;
            margin-left: 8px;
        }

        .confidence-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--gray-300);
        }

        .confidence-high .confidence-dot {
            background: var(--secondary);
        }

        .confidence-medium .confidence-dot:nth-child(-n+2) {
            background: var(--warning);
        }

        .confidence-low .confidence-dot:first-child {
            background: var(--danger);
        }

        .loading-spinner {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(0,0,0,.1);
            border-top-color: var(--primary);
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        @media (max-width: 1024px) {
            .main-content {
                grid-template-columns: 1fr;
            }
            
            .archetype-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🚀 SOW Builder - Live Data Integration</h1>
            <p>Create accurate Statements of Work based on your actual historical project data</p>
        </div>

        <!-- Data Upload Section -->
        <div class="data-upload-section">
            <h2 style="margin-bottom: 16px;">📊 Connect Your Data Source</h2>
            <div class="upload-area" id="uploadArea">
                <input type="file" id="fileInput" accept=".xlsx,.xls,.csv">
                <div class="upload-icon">📁</div>
                <div class="upload-text">Drop your Excel file here or click to browse</div>
                <div class="upload-subtext">Supports .xlsx, .xls, .csv files</div>
            </div>
            
            <div id="dataStatus" class="data-status"></div>
            
            <div id="dataStats" class="data-stats" style="display: none;">
                <div class="stat-card">
                    <div class="stat-label">Total Projects</div>
                    <div class="stat-value" id="statProjects">0</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Unique Clients</div>
                    <div class="stat-value" id="statClients">0</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Archetypes</div>
                    <div class="stat-value" id="statArchetypes">0</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Total Value</div>
                    <div class="stat-value" id="statValue">$0</div>
                </div>
            </div>
        </div>

        <div class="main-content" id="mainContent" style="display: none;">
            <div class="form-section">
                <div class="step-indicator">
                    <div class="step active" data-step="1">
                        <div class="step-number">1</div>
                        <div class="step-label">Client & Basic Info</div>
                    </div>
                    <div class="step" data-step="2">
                        <div class="step-number">2</div>
                        <div class="step-label">Select Archetype</div>
                    </div>
                    <div class="step" data-step="3">
                        <div class="step-number">3</div>
                        <div class="step-label">Project Details</div>
                    </div>
                    <div class="step" data-step="4">
                        <div class="step-number">4</div>
                        <div class="step-label">Resources & Timeline</div>
                    </div>
                    <div class="step" data-step="5">
                        <div class="step-number">5</div>
                        <div class="step-label">Review & Generate</div>
                    </div>
                </div>

                <!-- Step 1: Client & Basic Info -->
                <div id="step1" class="step-content">
                    <h2>Client & Basic Information <span class="live-data-badge">LIVE DATA</span></h2>
                    
                    <div class="form-group">
                        <label>Client*</label>
                        <select id="client">
                            <option value="">Select a client...</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label>Brand/Product</label>
                        <select id="brand">
                            <option value="">Select a brand...</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label>Portfolio*</label>
                        <select id="portfolio">
                            <option value="">Select portfolio...</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label>SOW Year*</label>
                        <select id="sowYear">
                            <option value="2025">2025</option>
                            <option value="2026">2026</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label>Project Name*</label>
                        <input type="text" id="projectName" placeholder="e.g., HCP Email Campaign Q1 2025">
                    </div>
                </div>

                <!-- Step 2: Select Archetype -->
                <div id="step2" class="step-content" style="display: none;">
                    <h2>Select Project Archetype <span class="live-data-badge">LIVE DATA</span></h2>
                    <p style="color: var(--gray-600); margin-bottom: 20px;">Statistics shown are from your actual project data</p>
                    
                    <div class="archetype-grid" id="archetypeGrid">
                        <!-- Will be populated dynamically from data -->
                    </div>
                </div>

                <!-- Step 3: Project Details -->
                <div id="step3" class="step-content" style="display: none;">
                    <h2>Project Details</h2>
                    
                    <div class="form-group">
                        <label>Project Description*</label>
                        <textarea id="description" placeholder="Describe the project scope and objectives..."></textarea>
                    </div>

                    <div class="form-group">
                        <label>Key Deliverables <span class="live-data-badge">AUTO-POPULATED</span></label>
                        <div id="deliverables-list">
                            <!-- Will be populated based on archetype -->
                        </div>
                    </div>

                    <div class="form-group">
                        <label>Assumptions & Exclusions</label>
                        <textarea id="assumptions" placeholder="List key assumptions and exclusions..."></textarea>
                    </div>
                </div>

                <!-- Step 4: Resources & Timeline -->
                <div id="step4" class="step-content" style="display: none;">
                    <h2>Resources & Timeline</h2>
                    
                    <div class="historical-data">
                        <h4>📊 Historical Data Analysis <span class="live-data-badge">LIVE</span></h4>
                        <div id="historical-stats">
                            <!-- Will be populated dynamically -->
                        </div>
                    </div>

                    <div class="form-group">
                        <label>Estimated Hours* <span class="suggested-value" id="suggest-hours">Use AI Suggestion</span></label>
                        <input type="number" id="hours" placeholder="Enter estimated hours">
                    </div>

                    <div class="form-group">
                        <label>Project Fee* <span class="suggested-value" id="suggest-fee">Use AI Suggestion</span></label>
                        <input type="number" id="fee" placeholder="Enter project fee">
                    </div>

                    <div class="form-group">
                        <label>Out of Pocket (OOP)</label>
                        <input type="number" id="oop" placeholder="Enter OOP expenses if applicable">
                    </div>

                    <div class="form-group">
                        <label>Timeline (Weeks)</label>
                        <input type="number" id="timeline" placeholder="Enter project timeline in weeks">
                    </div>

                    <div class="form-group">
                        <label>Team Composition</label>
                        <div id="team-composition">
                            <!-- Will be populated based on archetype -->
                        </div>
                    </div>
                </div>

                <!-- Step 5: Review & Generate -->
                <div id="step5" class="step-content" style="display: none;">
                    <h2>Review & Generate SOW</h2>
                    
                    <div class="preview-section">
                        <h3>SOW Preview</h3>
                        <div id="sow-preview" class="preview-content">
                            <!-- Will be populated with SOW preview -->
                        </div>
                    </div>
                </div>

                <div class="button-group">
                    <button class="btn btn-secondary" id="btn-back" style="display: none;">← Back</button>
                    <button class="btn btn-primary" id="btn-next">Next →</button>
                    <button class="btn btn-success" id="btn-generate" style="display: none;">Generate SOW</button>
                </div>
            </div>

            <div class="sidebar">
                <h3>💡 Smart Suggestions <span class="live-data-badge">LIVE</span></h3>
                <div id="smart-suggestions">
                    <p style="color: var(--gray-600); font-size: 13px;">
                        Powered by your historical project data
                    </p>
                </div>

                <div class="similar-projects">
                    <h3>📋 Similar Past Projects</h3>
                    <div id="similar-projects-list">
                        <!-- Will be populated dynamically from real data -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Global variables for data storage
        let projectData = [];
        let clientData = {};
        let archetypeStats = {};
        let currentStep = 1;
        let sowData = {
            client: '',
            brand: '',
            portfolio: '',
            sowYear: '2025',
            projectName: '',
            archetype: '',
            description: '',
            deliverables: [],
            assumptions: '',
            hours: 0,
            fee: 0,
            oop: 0,
            timeline: 0,
            team: []
        };

        // Archetype mapping based on project types
        const archetypeMapping = {
            'account-management': ['Account', 'Management Oversight', 'Brand Ops', 'Account Mgmt'],
            'digital-asset': ['Website', 'Landing Page', 'Microsite', 'Digital Asset', 'iVA'],
            'email-marketing': ['Email', 'CRM', 'Salesforce', 'Marketing Automation'],
            'video-production': ['Video', 'Animation', 'Filming', 'MOA', 'KOL Video'],
            'social-media': ['Social', 'Facebook', 'LinkedIn', 'Twitter', 'Instagram'],
            'print-materials': ['Print', 'Brochure', 'Leave Behind', 'Sales Aid', 'Flashcard'],
            'media-planning': ['Media Planning', 'Media Management', 'Media Buy', 'Media Setup'],
            'analytics': ['Analytics', 'Reporting', 'Dashboard', 'Performance', 'KPI'],
            'strategic-consulting': ['Strategy', 'Strategic', 'Workshop', 'Planning', 'Consultation'],
            'creative-concept': ['Creative', 'Concept', 'Ideation', 'Messaging'],
            'conference-event': ['Conference', 'Event', 'NSM', 'Symposium', 'Booth'],
            'market-research': ['Research', 'Ad Board', 'Journey', 'Insights', 'Market']
        };

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            setupEventListeners();
        });

        function setupEventListeners() {
            // File upload
            const uploadArea = document.getElementById('uploadArea');
            const fileInput = document.getElementById('fileInput');

            uploadArea.addEventListener('click', () => fileInput.click());
            uploadArea.addEventListener('dragover', handleDragOver);
            uploadArea.addEventListener('dragleave', handleDragLeave);
            uploadArea.addEventListener('drop', handleDrop);
            fileInput.addEventListener('change', handleFileSelect);

            // Navigation
            document.getElementById('btn-next').addEventListener('click', nextStep);
            document.getElementById('btn-back').addEventListener('click', previousStep);
            document.getElementById('btn-generate').addEventListener('click', generateSOW);

            // Client change
            document.getElementById('client').addEventListener('change', updateClientRelatedData);

            // Auto-fill suggestions
            document.getElementById('suggest-hours').addEventListener('click', autoFillHours);
            document.getElementById('suggest-fee').addEventListener('click', autoFillFee);
        }

        // File handling functions
        function handleDragOver(e) {
            e.preventDefault();
            e.currentTarget.classList.add('dragover');
        }

        function handleDragLeave(e) {
            e.currentTarget.classList.remove('dragover');
        }

        function handleDrop(e) {
            e.preventDefault();
            e.currentTarget.classList.remove('dragover');
            
            const files = e.dataTransfer.files;
            if (files.length > 0) {
                processFile(files[0]);
            }
        }

        function handleFileSelect(e) {
            const file = e.target.files[0];
            if (file) {
                processFile(file);
            }
        }

        function processFile(file) {
            const reader = new FileReader();
            const uploadArea = document.getElementById('uploadArea');
            const dataStatus = document.getElementById('dataStatus');
            
            // Show loading state
            dataStatus.className = 'data-status';
            dataStatus.innerHTML = '<div class="loading-spinner"></div> Processing file...';
            dataStatus.style.display = 'block';

            reader.onload = function(e) {
                try {
                    const data = new Uint8Array(e.target.result);
                    const workbook = XLSX.read(data, { type: 'array' });
                    
                    // Try to find the correct sheet
                    let sheetName = 'Full Data';
                    if (!workbook.Sheets[sheetName]) {
                        sheetName = 'ALL Data';
                        if (!workbook.Sheets[sheetName]) {
                            sheetName = workbook.SheetNames[0];
                        }
                    }
                    
                    const worksheet = workbook.Sheets[sheetName];
                    projectData = XLSX.utils.sheet_to_json(worksheet);
                    
                    if (projectData.length === 0) {
                        throw new Error('No data found in the Excel file');
                    }
                    
                    // Process and analyze data
                    analyzeProjectData();
                    
                    // Update UI
                    uploadArea.classList.add('has-data');
                    dataStatus.className = 'data-status success';
                    dataStatus.innerHTML = `✓ Successfully loaded ${projectData.length} projects from ${file.name}`;
                    
                    // Show stats
                    displayDataStats();
                    
                    // Populate form fields
                    populateFormFields();
                    
                    // Show main content
                    document.getElementById('mainContent').style.display = 'grid';
                    
                } catch (error) {
                    console.error('Error processing file:', error);
                    dataStatus.className = 'data-status error';
                    dataStatus.innerHTML = `✗ Error: ${error.message}`;
                }
            };

            reader.readAsArrayBuffer(file);
        }

        function analyzeProjectData() {
            // Reset data structures
            clientData = {};
            archetypeStats = {};
            
            // Analyze by client
            projectData.forEach(project => {
                const client = project['Client'];
                const fee = parseFloat(project['Fee']) || 0;
                const hours = parseFloat(project['Hours']) || 0;
                const type = project['Expected Type'] || project['Suggested Type'] || '';
                
                if (client) {
                    if (!clientData[client]) {
                        clientData[client] = {
                            projects: [],
                            totalFee: 0,
                            totalHours: 0,
                            brands: new Set(),
                            types: {}
                        };
                    }
                    
                    clientData[client].projects.push(project);
                    clientData[client].totalFee += fee;
                    clientData[client].totalHours += hours;
                    
                    if (project['Brand']) {
                        clientData[client].brands.add(project['Brand']);
                    }
                    
                    // Count project types
                    if (type) {
                        clientData[client].types[type] = (clientData[client].types[type] || 0) + 1;
                    }
                }
                
                // Analyze by archetype
                const archetype = detectArchetype(project);
                if (archetype) {
                    if (!archetypeStats[archetype]) {
                        archetypeStats[archetype] = {
                            projects: [],
                            totalFee: 0,
                            totalHours: 0,
                            minHours: Infinity,
                            maxHours: 0,
                            minFee: Infinity,
                            maxFee: 0
                        };
                    }
                    
                    archetypeStats[archetype].projects.push(project);
                    archetypeStats[archetype].totalFee += fee;
                    archetypeStats[archetype].totalHours += hours;
                    
                    if (hours > 0) {
                        archetypeStats[archetype].minHours = Math.min(archetypeStats[archetype].minHours, hours);
                        archetypeStats[archetype].maxHours = Math.max(archetypeStats[archetype].maxHours, hours);
                    }
                    
                    if (fee > 0) {
                        archetypeStats[archetype].minFee = Math.min(archetypeStats[archetype].minFee, fee);
                        archetypeStats[archetype].maxFee = Math.max(archetypeStats[archetype].maxFee, fee);
                    }
                }
            });
        }

        function detectArchetype(project) {
            const projectName = (project['Project Name'] || '').toLowerCase();
            const type = ((project['Expected Type'] || '') + ' ' + (project['Suggested Type'] || '')).toLowerCase();
            const description = (project['Description'] || '').toLowerCase();
            const combined = projectName + ' ' + type + ' ' + description;
            
            for (const [archetype, keywords] of Object.entries(archetypeMapping)) {
                for (const keyword of keywords) {
                    if (combined.includes(keyword.toLowerCase())) {
                        return archetype;
                    }
                }
            }
            
            return 'other';
        }

        function displayDataStats() {
            const stats = document.getElementById('dataStats');
            stats.style.display = 'grid';
            
            // Calculate stats
            const totalProjects = projectData.length;
            const uniqueClients = Object.keys(clientData).length;
            const uniqueArchetypes = Object.keys(archetypeStats).length;
            const totalValue = projectData.reduce((sum, p) => sum + (parseFloat(p['Total']) || 0), 0);
            
            document.getElementById('statProjects').textContent = totalProjects.toLocaleString();
            document.getElementById('statClients').textContent = uniqueClients;
            document.getElementById('statArchetypes').textContent = uniqueArchetypes;
            document.getElementById('statValue').textContent = `$${Math.round(totalValue / 1000000)}M`;
        }

        function populateFormFields() {
            // Populate clients dropdown
            const clientSelect = document.getElementById('client');
            clientSelect.innerHTML = '<option value="">Select a client...</option>';
            
            Object.keys(clientData).sort().forEach(client => {
                const option = document.createElement('option');
                option.value = client;
                option.textContent = `${client} (${clientData[client].projects.length} projects)`;
                clientSelect.appendChild(option);
            });
            
            // Populate portfolios
            const portfolios = [...new Set(projectData.map(p => p['Portfolio']).filter(Boolean))];
            const portfolioSelect = document.getElementById('portfolio');
            portfolioSelect.innerHTML = '<option value="">Select portfolio...</option>';
            
            portfolios.forEach(portfolio => {
                const option = document.createElement('option');
                option.value = portfolio;
                option.textContent = portfolio;
                portfolioSelect.appendChild(option);
            });
            
            // Populate archetype cards with real data
            populateArchetypes();
        }

        function populateArchetypes() {
            const archetypeGrid = document.getElementById('archetypeGrid');
            archetypeGrid.innerHTML = '';
            
            const archetypeDetails = {
                'account-management': { name: 'Account & Brand Management', desc: 'Ongoing account oversight' },
                'digital-asset': { name: 'Digital Asset Creation', desc: 'Websites, landing pages' },
                'email-marketing': { name: 'Email Marketing', desc: 'Email campaigns, automation' },
                'video-production': { name: 'Video Production', desc: 'Video content, animation' },
                'social-media': { name: 'Social Media', desc: 'Social content & management' },
                'print-materials': { name: 'Traditional Print', desc: 'Brochures, sales aids' },
                'media-planning': { name: 'Media Planning', desc: 'Media strategy & buying' },
                'analytics': { name: 'Analytics & Reporting', desc: 'Performance measurement' },
                'strategic-consulting': { name: 'Strategic Consulting', desc: 'Strategy & planning' },
                'creative-concept': { name: 'Creative Concept', desc: 'Ideation & concepting' },
                'conference-event': { name: 'Conference & Events', desc: 'Event support materials' },
                'market-research': { name: 'Market Research', desc: 'Insights & research' }
            };
            
            Object.entries(archetypeDetails).forEach(([key, details]) => {
                const stats = archetypeStats[key];
                if (stats && stats.projects.length > 0) {
                    const avgHours = Math.round(stats.totalHours / stats.projects.length);
                    const avgFee = Math.round(stats.totalFee / stats.projects.length);
                    
                    const card = document.createElement('div');
                    card.className = 'archetype-card';
                    card.dataset.archetype = key;
                    card.innerHTML = `
                        <div class="archetype-title">${details.name}</div>
                        <div class="archetype-desc">${details.desc}</div>
                        <div class="archetype-stats live-data">
                            <span>${stats.projects.length} projects</span>
                            <span>Avg: ${avgHours} hrs</span>
                            <span>$${(avgFee/1000).toFixed(0)}K</span>
                        </div>
                    `;
                    card.addEventListener('click', selectArchetype);
                    archetypeGrid.appendChild(card);
                }
            });
        }

        function updateClientRelatedData() {
            const selectedClient = document.getElementById('client').value;
            
            if (selectedClient && clientData[selectedClient]) {
                const client = clientData[selectedClient];
                
                // Update brands dropdown
                const brandSelect = document.getElementById('brand');
                brandSelect.innerHTML = '<option value="">Select a brand...</option>';
                
                client.brands.forEach(brand => {
                    const option = document.createElement('option');
                    option.value = brand;
                    option.textContent = brand;
                    brandSelect.appendChild(option);
                });
                
                // Update suggestions
                updateClientSuggestions(client);
                
                // Show similar projects
                showSimilarProjects(client.projects.slice(0, 5));
            }
        }

        function updateClientSuggestions(client) {
            const avgHours = Math.round(client.totalHours / client.projects.length);
            const avgFee = Math.round(client.totalFee / client.projects.length);
            
            let suggestionsHTML = `
                <div style="padding: 12px; background: #f0f9ff; border-radius: 8px; margin-bottom: 16px;">
                    <h4 style="font-size: 14px; font-weight: 600; margin-bottom: 8px;">
                        Client Analytics
                    </h4>
                    <div style="font-size: 12px; color: #4b5563;">
                        <p>📊 ${client.projects.length} past projects</p>
                        <p>⏱ Avg: ${avgHours} hours</p>
                        <p>💰 Avg: $${avgFee.toLocaleString()}</p>
                        <p>🏷 ${client.brands.size} brands</p>
                    </div>
                </div>
            `;
            
            // Show top project types
            const topTypes = Object.entries(client.types)
                .sort((a, b) => b[1] - a[1])
                .slice(0, 3);
            
            if (topTypes.length > 0) {
                suggestionsHTML += `
                    <div style="padding: 12px; background: #fef3c7; border-radius: 8px;">
                        <h4 style="font-size: 14px; font-weight: 600; margin-bottom: 8px;">
                            Common Project Types
                        </h4>
                        <div style="font-size: 12px; color: #78350f;">
                            ${topTypes.map(([type, count]) => `<p>${type}: ${count}</p>`).join('')}
                        </div>
                    </div>
                `;
            }
            
            document.getElementById('smart-suggestions').innerHTML = suggestionsHTML;
        }

        function selectArchetype(e) {
            document.querySelectorAll('.archetype-card').forEach(card => {
                card.classList.remove('selected');
            });
            
            e.currentTarget.classList.add('selected');
            sowData.archetype = e.currentTarget.dataset.archetype;
        }

        function showSimilarProjects(projects) {
            let html = '';
            
            projects.forEach(project => {
                const name = project['Project Name'] || 'Unnamed Project';
                const client = project['Client'];
                const hours = project['Hours'] || 0;
                const fee = project['Fee'] || 0;
                
                html += `
                    <div class="project-item" onclick="useSimilarProject(${projectData.indexOf(project)})">
                        <div class="project-name">${name}</div>
                        <div class="project-details">
                            ${client} • ${hours} hrs • $${fee.toLocaleString()}
                        </div>
                    </div>
                `;
            });
            
            document.getElementById('similar-projects-list').innerHTML = html || '<p style="color: #9ca3af; font-size: 12px;">No similar projects found</p>';
        }

        function useSimilarProject(index) {
            const project = projectData[index];
            if (project) {
                // Auto-fill form fields with similar project data
                if (project['Description']) {
                    document.getElementById('description').value = `Similar to: ${project['Description']}`;
                }
                if (project['Hours']) {
                    document.getElementById('hours').value = project['Hours'];
                }
                if (project['Fee']) {
                    document.getElementById('fee').value = project['Fee'];
                }
                
                alert('Project data loaded! Please review and adjust as needed.');
            }
        }

        function populateDeliverables() {
            if (sowData.archetype && archetypeStats[sowData.archetype]) {
                const projects = archetypeStats[sowData.archetype].projects;
                
                // Extract common deliverables from descriptions
                const commonPhrases = new Set();
                projects.forEach(project => {
                    const desc = project['Description'] || '';
                    const assumptions = project['Assumptions/Exclusions'] || '';
                    
                    // Extract bullet points or key phrases
                    const bullets = (desc + ' ' + assumptions).match(/[•\-\>]\s*([^\n\r•\-\>]+)/g) || [];
                    bullets.forEach(bullet => {
                        const cleaned = bullet.replace(/[•\-\>]\s*/, '').trim();
                        if (cleaned.length > 10 && cleaned.length < 100) {
                            commonPhrases.add(cleaned);
                        }
                    });
                });
                
                let html = '';
                const deliverables = Array.from(commonPhrases).slice(0, 8);
                
                if (deliverables.length === 0) {
                    // Use default deliverables if none found
                    deliverables.push(
                        'Project kickoff and planning',
                        'Creative development',
                        'Review rounds (up to 3)',
                        'Final deliverable production',
                        'Project documentation'
                    );
                }
                
                deliverables.forEach((deliverable, index) => {
                    html += `
                        <div class="deliverable-item">
                            <input type="checkbox" id="del-${index}" value="${deliverable}" checked>
                            <label for="del-${index}">${deliverable}</label>
                        </div>
                    `;
                });
                
                document.getElementById('deliverables-list').innerHTML = html;
            }
        }

        function showHistoricalData() {
            if (sowData.archetype && archetypeStats[sowData.archetype]) {
                const stats = archetypeStats[sowData.archetype];
                const avgHours = Math.round(stats.totalHours / stats.projects.length);
                const avgFee = Math.round(stats.totalFee / stats.projects.length);
                
                // Calculate percentiles
                const hoursSorted = stats.projects.map(p => p['Hours']).filter(h => h > 0).sort((a, b) => a - b);
                const p25Hours = hoursSorted[Math.floor(hoursSorted.length * 0.25)];
                const p75Hours = hoursSorted[Math.floor(hoursSorted.length * 0.75)];
                
                const feesSorted = stats.projects.map(p => p['Fee']).filter(f => f > 0).sort((a, b) => a - b);
                const p25Fee = feesSorted[Math.floor(feesSorted.length * 0.25)];
                const p75Fee = feesSorted[Math.floor(feesSorted.length * 0.75)];
                
                let html = `
                    <div class="data-row">
                        <span class="data-label">Sample Size:</span>
                        <span class="data-value">${stats.projects.length} projects</span>
                    </div>
                    <div class="data-row">
                        <span class="data-label">Average:</span>
                        <span class="data-value">${avgHours} hrs / $${avgFee.toLocaleString()}</span>
                    </div>
                    <div class="data-row">
                        <span class="data-label">25th-75th Percentile Hours:</span>
                        <span class="data-value">${p25Hours}-${p75Hours} hrs</span>
                    </div>
                    <div class="data-row">
                        <span class="data-label">25th-75th Percentile Fee:</span>
                        <span class="data-value">$${p25Fee.toLocaleString()}-$${p75Fee.toLocaleString()}</span>
                    </div>
                    <div class="data-row">
                        <span class="data-label">Confidence Level:</span>
                        <span class="confidence-indicator ${stats.projects.length > 10 ? 'confidence-high' : stats.projects.length > 5 ? 'confidence-medium' : 'confidence-low'}">
                            <span class="confidence-dot"></span>
                            <span class="confidence-dot"></span>
                            <span class="confidence-dot"></span>
                            <span style="font-size: 11px;">${stats.projects.length > 10 ? 'High' : stats.projects.length > 5 ? 'Medium' : 'Low'}</span>
                        </span>
                    </div>
                `;
                
                document.getElementById('historical-stats').innerHTML = html;
                
                // Set suggested values
                document.getElementById('hours').placeholder = `Suggested: ${avgHours}`;
                document.getElementById('fee').placeholder = `Suggested: $${avgFee}`;
                
                // Show similar archetype projects
                showSimilarProjects(stats.projects.slice(0, 5));
            }
        }

        function autoFillHours() {
            if (sowData.archetype && archetypeStats[sowData.archetype]) {
                const stats = archetypeStats[sowData.archetype];
                const avgHours = Math.round(stats.totalHours / stats.projects.length);
                document.getElementById('hours').value = avgHours;
            }
        }

        function autoFillFee() {
            if (sowData.archetype && archetypeStats[sowData.archetype]) {
                const stats = archetypeStats[sowData.archetype];
                const avgFee = Math.round(stats.totalFee / stats.projects.length);
                document.getElementById('fee').value = avgFee;
            }
        }

        // Navigation functions
        function nextStep() {
            if (validateStep(currentStep)) {
                saveStepData(currentStep);
                
                if (currentStep < 5) {
                    document.getElementById(`step${currentStep}`).style.display = 'none';
                    currentStep++;
                    document.getElementById(`step${currentStep}`).style.display = 'block';
                    
                    updateStepIndicator();
                    updateButtons();
                    
                    if (currentStep === 3) {
                        populateDeliverables();
                    } else if (currentStep === 4) {
                        showHistoricalData();
                    } else if (currentStep === 5) {
                        generatePreview();
                    }
                }
            }
        }

        function previousStep() {
            if (currentStep > 1) {
                document.getElementById(`step${currentStep}`).style.display = 'none';
                currentStep--;
                document.getElementById(`step${currentStep}`).style.display = 'block';
                
                updateStepIndicator();
                updateButtons();
            }
        }

        function updateStepIndicator() {
            document.querySelectorAll('.step').forEach((step, index) => {
                if (index + 1 < currentStep) {
                    step.classList.add('completed');
                    step.classList.remove('active');
                } else if (index + 1 === currentStep) {
                    step.classList.add('active');
                    step.classList.remove('completed');
                } else {
                    step.classList.remove('active', 'completed');
                }
            });
        }

        function updateButtons() {
            document.getElementById('btn-back').style.display = currentStep > 1 ? 'block' : 'none';
            document.getElementById('btn-next').style.display = currentStep < 5 ? 'block' : 'none';
            document.getElementById('btn-generate').style.display = currentStep === 5 ? 'block' : 'none';
        }

        function validateStep(step) {
            switch(step) {
                case 1:
                    if (!document.getElementById('client').value) {
                        alert('Please select a client');
                        return false;
                    }
                    if (!document.getElementById('portfolio').value) {
                        alert('Please select a portfolio');
                        return false;
                    }
                    if (!document.getElementById('projectName').value) {
                        alert('Please enter a project name');
                        return false;
                    }
                    break;
                case 2:
                    if (!sowData.archetype) {
                        alert('Please select a project archetype');
                        return false;
                    }
                    break;
                case 3:
                    if (!document.getElementById('description').value) {
                        alert('Please enter a project description');
                        return false;
                    }
                    break;
                case 4:
                    if (!document.getElementById('hours').value) {
                        alert('Please enter estimated hours');
                        return false;
                    }
                    if (!document.getElementById('fee').value) {
                        alert('Please enter project fee');
                        return false;
                    }
                    break;
            }
            return true;
        }

        function saveStepData(step) {
            switch(step) {
                case 1:
                    sowData.client = document.getElementById('client').value;
                    sowData.brand = document.getElementById('brand').value;
                    sowData.portfolio = document.getElementById('portfolio').value;
                    sowData.sowYear = document.getElementById('sowYear').value;
                    sowData.projectName = document.getElementById('projectName').value;
                    break;
                case 3:
                    sowData.description = document.getElementById('description').value;
                    sowData.assumptions = document.getElementById('assumptions').value;
                    sowData.deliverables = [];
                    document.querySelectorAll('#deliverables-list input[type="checkbox"]:checked').forEach(cb => {
                        sowData.deliverables.push(cb.value);
                    });
                    break;
                case 4:
                    sowData.hours = parseInt(document.getElementById('hours').value);
                    sowData.fee = parseInt(document.getElementById('fee').value);
                    sowData.oop = parseInt(document.getElementById('oop').value || 0);
                    sowData.timeline = parseInt(document.getElementById('timeline').value || 0);
                    break;
            }
        }

        function generatePreview() {
            saveStepData(4);
            
            const total = sowData.fee + sowData.oop;
            const dataSourceInfo = projectData.length > 0 ? 
                `<div style="padding: 8px; background: #dcfce7; border-radius: 4px; margin-bottom: 16px;">
                    <strong style="color: #166534;">📊 Generated from ${projectData.length} historical projects</strong>
                </div>` : '';
            
            let preview = `
                ${dataSourceInfo}
                <div style="margin-bottom: 24px;">
                    <strong>Statement of Work</strong><br>
                    ${sowData.client} - ${sowData.brand || 'TBD'}<br>
                    ${sowData.projectName}
                </div>
                
                <div style="margin-bottom: 20px;">
                    <strong>Project Type:</strong> ${sowData.archetype}<br>
                    <strong>Portfolio:</strong> ${sowData.portfolio}<br>
                    <strong>SOW Year:</strong> ${sowData.sowYear}
                </div>
                
                <div style="margin-bottom: 20px;">
                    <strong>Project Description:</strong><br>
                    ${sowData.description}
                </div>
                
                <div style="margin-bottom: 20px;">
                    <strong>Deliverables:</strong><br>
                    <ul style="margin-left: 20px; margin-top: 8px;">
                        ${sowData.deliverables.map(d => `<li>${d}</li>`).join('')}
                    </ul>
                </div>
                
                ${sowData.assumptions ? `
                <div style="margin-bottom: 20px;">
                    <strong>Assumptions & Exclusions:</strong><br>
                    ${sowData.assumptions}
                </div>
                ` : ''}
                
                <div style="margin-bottom: 20px;">
                    <strong>Investment Summary:</strong><br>
                    <div style="margin-left: 20px; margin-top: 8px;">
                        Hours: ${sowData.hours}<br>
                        Professional Fees: $${sowData.fee.toLocaleString()}<br>
                        ${sowData.oop ? `Out of Pocket: $${sowData.oop.toLocaleString()}<br>` : ''}
                        <strong>Total: $${total.toLocaleString()}</strong>
                    </div>
                </div>
                
                <div style="margin-bottom: 20px;">
                    <strong>Timeline:</strong> ${sowData.timeline ? `${sowData.timeline} weeks` : 'TBD'}
                </div>
                
                <div style="padding: 12px; background: #dcfce7; border-radius: 8px; margin-top: 24px;">
                    <strong style="color: #166534;">✓ SOW Ready for Export</strong><br>
                    <span style="font-size: 12px; color: #15803d;">
                        Generated using live data analysis and AI recommendations
                    </span>
                </div>
            `;
            
            document.getElementById('sow-preview').innerHTML = preview;
        }

        function generateSOW() {
            // Create downloadable SOW
            const sowContent = document.getElementById('sow-preview').innerText;
            const blob = new Blob([sowContent], { type: 'text/plain' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `SOW_${sowData.client}_${sowData.projectName.replace(/\s+/g, '_')}.txt`;
            a.click();
            
            alert('SOW Generated and Downloaded! In production, this would also:\n• Export to PDF/Word format\n• Save to database\n• Create project in PM system\n• Send notifications to team');
        }
    </script>
</body>
</html>
