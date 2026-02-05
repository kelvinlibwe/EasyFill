# EasyFill
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EasyFill - Fill and Download Documents Hassle-Free</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf-lib/1.17.1/pdf-lib.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            color: white;
            margin-bottom: 40px;
        }

        .header h1 {
            font-size: 3em;
            margin-bottom: 10px;
            font-weight: 700;
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .main-card {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            overflow: hidden;
        }

        .upload-section {
            padding: 60px 40px;
            text-align: center;
            background: linear-gradient(to bottom, #f8f9ff 0%, white 100%);
        }

        .upload-area {
            border: 3px dashed #667eea;
            border-radius: 15px;
            padding: 60px 40px;
            transition: all 0.3s ease;
            cursor: pointer;
            background: white;
        }

        .upload-area:hover {
            border-color: #764ba2;
            background: #f8f9ff;
            transform: translateY(-2px);
        }

        .upload-area.dragover {
            border-color: #764ba2;
            background: #f0f1ff;
            transform: scale(1.02);
        }

        .upload-icon {
            font-size: 64px;
            margin-bottom: 20px;
            color: #667eea;
        }

        .upload-text {
            font-size: 1.3em;
            color: #333;
            margin-bottom: 10px;
            font-weight: 600;
        }

        .upload-subtext {
            color: #666;
            font-size: 1em;
        }

        .file-input {
            display: none;
        }

        .form-section {
            display: none;
            padding: 40px;
        }

        .form-section.active {
            display: block;
        }

        .user-info-form {
            background: #f8f9ff;
            padding: 30px;
            border-radius: 15px;
            margin-bottom: 30px;
        }

        .form-title {
            font-size: 1.5em;
            color: #333;
            margin-bottom: 20px;
            font-weight: 600;
        }

        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
        }

        .form-group label {
            font-size: 0.9em;
            color: #555;
            margin-bottom: 8px;
            font-weight: 500;
        }

        .form-group input {
            padding: 12px 15px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 1em;
            transition: all 0.3s ease;
        }

        .form-group input:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .btn {
            padding: 14px 32px;
            border: none;
            border-radius: 10px;
            font-size: 1.1em;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            gap: 10px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(102, 126, 234, 0.4);
        }

        .btn-secondary {
            background: #f0f0f0;
            color: #333;
        }

        .btn-secondary:hover {
            background: #e0e0e0;
        }

        .btn-success {
            background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%);
            color: white;
        }

        .btn-success:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(56, 239, 125, 0.4);
        }

        .pdf-viewer {
            margin-top: 30px;
            border: 2px solid #e0e0e0;
            border-radius: 15px;
            overflow: hidden;
            background: #f5f5f5;
        }

        .pdf-canvas-container {
            max-height: 600px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            gap: 20px;
        }

        .pdf-page {
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            background: white;
            margin-bottom: 10px;
        }

        .action-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 30px;
            flex-wrap: wrap;
        }

        .fields-detected {
            background: #e8f5e9;
            border-left: 4px solid #4caf50;
            padding: 15px 20px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-size: 0.95em;
            color: #2e7d32;
        }

        .privacy-notice {
            background: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 15px 20px;
            border-radius: 8px;
            margin-top: 20px;
            font-size: 0.9em;
            color: #856404;
        }

        .loading {
            display: none;
            text-align: center;
            padding: 40px;
        }

        .loading.active {
            display: block;
        }

        .spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #667eea;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
            margin: 0 auto 20px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .feature-list {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 40px;
            padding: 0 20px;
        }

        .feature-item {
            text-align: center;
            color: white;
        }

        .feature-icon {
            font-size: 2.5em;
            margin-bottom: 10px;
        }

        .feature-text {
            font-size: 0.95em;
            opacity: 0.9;
        }

        @media (max-width: 768px) {
            .header h1 {
                font-size: 2em;
            }

            .upload-area {
                padding: 40px 20px;
            }

            .form-section {
                padding: 20px;
            }

            .action-buttons {
                flex-direction: column;
            }

            .btn {
                width: 100%;
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>📄 EasyFill</h1>
            <p>Fill and download documents hassle-free!</p>
        </div>

        <div class="main-card">
            <!-- Upload Section -->
            <div class="upload-section" id="uploadSection">
                <div class="upload-area" id="uploadArea">
                    <div class="upload-icon">📁</div>
                    <div class="upload-text">Drag & Drop your PDF here</div>
                    <div class="upload-subtext">or click to browse</div>
                    <input type="file" id="fileInput" class="file-input" accept=".pdf">
                </div>
                <div class="privacy-notice">
                    🔒 <strong>100% Private:</strong> Your documents are processed entirely in your browser. Nothing is uploaded to any server.
                </div>
            </div>

            <!-- Loading -->
            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p>Processing your PDF...</p>
            </div>

            <!-- Form Section -->
            <div class="form-section" id="formSection">
                <div class="user-info-form">
                    <div class="form-title">📝 Your Information</div>
                    <div class="form-grid">
                        <div class="form-group">
                            <label for="fullName">Full Name</label>
                            <input type="text" id="fullName" placeholder="John Doe">
                        </div>
                        <div class="form-group">
                            <label for="email">Email Address</label>
                            <input type="email" id="email" placeholder="john@example.com">
                        </div>
                        <div class="form-group">
                            <label for="phone">Phone Number</label>
                            <input type="tel" id="phone" placeholder="+1 (555) 123-4567">
                        </div>
                        <div class="form-group">
                            <label for="address">Address</label>
                            <input type="text" id="address" placeholder="123 Main St, City, State">
                        </div>
                        <div class="form-group">
                            <label for="company">Company/Organization</label>
                            <input type="text" id="company" placeholder="Company Name">
                        </div>
                        <div class="form-group">
                            <label for="date">Date</label>
                            <input type="date" id="date">
                        </div>
                    </div>
                    <button class="btn btn-primary" id="autoFillBtn">
                        ✨ Auto-Fill Form
                    </button>
                </div>

                <div id="fieldsInfo"></div>

                <!-- PDF Viewer -->
                <div class="pdf-viewer">
                    <div class="pdf-canvas-container" id="pdfContainer"></div>
                </div>

                <!-- Action Buttons -->
                <div class="action-buttons">
                    <button class="btn btn-success" id="downloadBtn">
                        ⬇️ Download Filled PDF
                    </button>
                    <button class="btn btn-secondary" id="newDocBtn">
                        🔄 Upload New Document
                    </button>
                </div>
            </div>
        </div>

        <!-- Features -->
        <div class="feature-list">
            <div class="feature-item">
                <div class="feature-icon">🚀</div>
                <div class="feature-text">No signup required</div>
            </div>
            <div class="feature-item">
                <div class="feature-icon">🔓</div>
                <div class="feature-text">No watermarks</div>
            </div>
            <div class="feature-item">
                <div class="feature-icon">♾️</div>
                <div class="feature-text">Unlimited downloads</div>
            </div>
            <div class="feature-item">
                <div class="feature-icon">🔐</div>
                <div class="feature-text">100% secure & private</div>
            </div>
        </div>
    </div>

    <script>
        // Set PDF.js worker
        pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

        let pdfDoc = null;
        let pdfBytes = null;
        let formFields = [];

        // Elements
        const uploadArea = document.getElementById('uploadArea');
        const fileInput = document.getElementById('fileInput');
        const uploadSection = document.getElementById('uploadSection');
        const formSection = document.getElementById('formSection');
        const loading = document.getElementById('loading');
        const pdfContainer = document.getElementById('pdfContainer');
        const autoFillBtn = document.getElementById('autoFillBtn');
        const downloadBtn = document.getElementById('downloadBtn');
        const newDocBtn = document.getElementById('newDocBtn');
        const fieldsInfo = document.getElementById('fieldsInfo');

        // Set today's date
        document.getElementById('date').valueAsDate = new Date();

        // Upload area click
        uploadArea.addEventListener('click', () => fileInput.click());

        // Drag and drop
        uploadArea.addEventListener('dragover', (e) => {
            e.preventDefault();
            uploadArea.classList.add('dragover');
        });

        uploadArea.addEventListener('dragleave', () => {
            uploadArea.classList.remove('dragover');
        });

        uploadArea.addEventListener('drop', (e) => {
            e.preventDefault();
            uploadArea.classList.remove('dragover');
            const files = e.dataTransfer.files;
            if (files.length > 0 && files[0].type === 'application/pdf') {
                handleFile(files[0]);
            }
        });

        // File input change
        fileInput.addEventListener('change', (e) => {
            if (e.target.files.length > 0) {
                handleFile(e.target.files[0]);
            }
        });

        // Handle file upload
        async function handleFile(file) {
            uploadSection.style.display = 'none';
            loading.classList.add('active');

            try {
                const arrayBuffer = await file.arrayBuffer();
                pdfBytes = new Uint8Array(arrayBuffer);
                
                // Load with PDF-lib for form detection
                pdfDoc = await PDFLib.PDFDocument.load(pdfBytes);
                const form = pdfDoc.getForm();
                formFields = form.getFields();

                // Render preview with PDF.js
                await renderPDF(arrayBuffer);

                loading.classList.remove('active');
                formSection.classList.add('active');

                // Show fields info
                if (formFields.length > 0) {
                    fieldsInfo.innerHTML = `
                        <div class="fields-detected">
                            ✅ <strong>${formFields.length} form field(s) detected</strong> - Ready to auto-fill!
                        </div>
                    `;
                } else {
                    fieldsInfo.innerHTML = `
                        <div class="fields-detected" style="background: #fff3cd; border-color: #ffc107; color: #856404;">
                            ℹ️ No fillable fields detected. You can still download the PDF or try a different document.
                        </div>
                    `;
                }
            } catch (error) {
                console.error('Error processing PDF:', error);
                alert('Error processing PDF. Please try another file.');
                resetApp();
            }
        }

        // Render PDF preview
        async function renderPDF(arrayBuffer) {
            const loadingTask = pdfjsLib.getDocument({ data: arrayBuffer });
            const pdf = await loadingTask.promise;
            pdfContainer.innerHTML = '';

            // Render first 3 pages for preview
            const pagesToRender = Math.min(pdf.numPages, 3);
            
            for (let pageNum = 1; pageNum <= pagesToRender; pageNum++) {
                const page = await pdf.getPage(pageNum);
                const viewport = page.getViewport({ scale: 1.5 });
                
                const canvas = document.createElement('canvas');
                canvas.className = 'pdf-page';
                const context = canvas.getContext('2d');
                canvas.height = viewport.height;
                canvas.width = viewport.width;

                await page.render({
                    canvasContext: context,
                    viewport: viewport
                }).promise;

                pdfContainer.appendChild(canvas);
            }

            if (pdf.numPages > 3) {
                const morePages = document.createElement('div');
                morePages.style.padding = '20px';
                morePages.style.textAlign = 'center';
                morePages.style.color = '#666';
                morePages.textContent = `... and ${pdf.numPages - 3} more page(s)`;
                pdfContainer.appendChild(morePages);
            }
        }

        // Auto-fill form
        autoFillBtn.addEventListener('click', async () => {
            if (!pdfDoc || formFields.length === 0) {
                alert('No fillable fields found in this PDF.');
                return;
            }

            try {
                const form = pdfDoc.getForm();
                const userData = {
                    name: document.getElementById('fullName').value,
                    email: document.getElementById('email').value,
                    phone: document.getElementById('phone').value,
                    address: document.getElementById('address').value,
                    company: document.getElementById('company').value,
                    date: document.getElementById('date').value
                };

                // Try to fill fields based on common field names
                formFields.forEach(field => {
                    const fieldName = field.getName().toLowerCase();
                    
                    try {
                        if (field.constructor.name === 'PDFTextField') {
                            if (fieldName.includes('name') && !fieldName.includes('company')) {
                                field.setText(userData.name);
                            } else if (fieldName.includes('email') || fieldName.includes('e-mail')) {
                                field.setText(userData.email);
                            } else if (fieldName.includes('phone') || fieldName.includes('tel')) {
                                field.setText(userData.phone);
                            } else if (fieldName.includes('address')) {
                                field.setText(userData.address);
                            } else if (fieldName.includes('company') || fieldName.includes('organization')) {
                                field.setText(userData.company);
                            } else if (fieldName.includes('date')) {
                                field.setText(userData.date);
                            }
                        }
                    } catch (e) {
                        console.log('Could not fill field:', fieldName, e);
                    }
                });

                // Re-render the filled PDF
                const filledPdfBytes = await pdfDoc.save();
                await renderPDF(filledPdfBytes.buffer);
                pdfBytes = filledPdfBytes;

                alert('✅ Form auto-filled successfully! Review and download when ready.');
            } catch (error) {
                console.error('Error filling form:', error);
                alert('Some fields could not be filled automatically. Please review the PDF.');
            }
        });

        // Download PDF
        downloadBtn.addEventListener('click', async () => {
            try {
                let finalBytes = pdfBytes;
                
                // If we have a modified document, save it
                if (pdfDoc) {
                    finalBytes = await pdfDoc.save();
                }

                const blob = new Blob([finalBytes], { type: 'application/pdf' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `filled-document-${Date.now()}.pdf`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                URL.revokeObjectURL(url);

                alert('✅ PDF downloaded successfully!');
            } catch (error) {
                console.error('Error downloading PDF:', error);
                alert('Error downloading PDF. Please try again.');
            }
        });

        // Reset app
        newDocBtn.addEventListener('click', resetApp);

        function resetApp() {
            pdfDoc = null;
            pdfBytes = null;
            formFields = [];
            pdfContainer.innerHTML = '';
            fieldsInfo.innerHTML = '';
            fileInput.value = '';
            
            uploadSection.style.display = 'block';
            formSection.classList.remove('active');
            loading.classList.remove('active');

            // Clear form
            document.getElementById('fullName').value = '';
            document.getElementById('email').value = '';
            document.getElementById('phone').value = '';
            document.getElementById('address').value = '';
            document.getElementById('company').value = '';
            document.getElementById('date').valueAsDate = new Date();
        }
    </script>
</body>
</html> your PDF online
