<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GTG Staff - Applicant Intake Portal</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f9ff',
                            100: '#e0f2fe',
                            500: '#0284c7',
                            600: '#0369a1',
                            700: '#075985',
                            800: '#0c4a6e',
                            900: '#0f172a',
                        }
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-slate-100 text-slate-800 min-h-screen flex flex-col">

    <!-- Navigation Header -->
    <header class="bg-slate-900 text-white border-b border-slate-800 sticky top-0 z-40 shadow-md">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-brand-600 to-teal-500 text-white flex items-center justify-center font-bold text-xl shadow-inner">
                    <i class="fa-solid fa-clipboard-user text-lg"></i>
                </div>
                <div>
                    <h1 class="font-bold text-white text-base sm:text-lg leading-tight">PM Staff Intake Portal</h1>
                    <p class="text-xs text-slate-400">Boarding Home Candidate Application System</p>
                </div>
            </div>
            
            <!-- Navigation Tabs -->
            <div class="flex bg-slate-800 p-1 rounded-xl border border-slate-700 text-xs font-medium space-x-1">
                <button id="btn-tab-form" onclick="switchTab('form')" class="px-3 py-2 rounded-lg bg-brand-600 text-white shadow-sm transition flex items-center space-x-1.5 font-semibold">
                    <i class="fa-solid fa-pen-to-square"></i>
                    <span>Applicant Form</span>
                </button>
                <button id="btn-tab-guide" onclick="switchTab('guide')" class="px-3 py-2 rounded-lg text-slate-300 hover:text-white hover:bg-slate-700 transition flex items-center space-x-1.5">
                    <i class="fa-solid fa-table-cells"></i>
                    <span>Google Sheets Integration</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Notification Toast System -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 space-y-2 pointer-events-none"></div>

    <main class="max-w-5xl mx-auto px-4 py-6 sm:px-6 lg:px-8 flex-1 w-full">

        <!-- TAB 1: APPLICANT FORM -->
        <div id="tab-form" class="space-y-6">
            
            <!-- Webhook Connection Status Banner -->
            <div id="webhook-status-banner" class="bg-amber-50 border border-amber-200 text-amber-900 rounded-2xl p-4 text-xs sm:text-sm flex items-start space-x-3 shadow-sm">
                <i class="fa-solid fa-circle-info text-amber-600 text-xl mt-0.5 shrink-0"></i>
                <div class="flex-1 space-y-2">
                    <div class="flex items-center justify-between">
                        <span class="font-bold text-amber-900">Google Sheets Endpoint Target</span>
                        <span id="webhook-badge" class="text-[10px] font-semibold bg-amber-200/80 text-amber-900 px-2 py-0.5 rounded-md">Demo Mode</span>
                    </div>
                    <p class="text-amber-800 text-xs">Paste your Google Apps Script Web App URL below to automatically push all submissions directly to your Drive spreadsheet.</p>
                    <div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-2">
                        <input type="text" id="script-url-input" placeholder="https://script.google.com/macros/s/.../exec" class="flex-1 px-3 py-1.5 bg-white border border-amber-300 rounded-lg text-slate-800 focus:outline-none focus:ring-2 focus:ring-brand-500 text-xs">
                        <div class="flex items-center space-x-2">
                            <button onclick="saveWebhookUrl()" class="px-3 py-1.5 bg-amber-800 hover:bg-amber-900 text-white font-medium rounded-lg text-xs transition shrink-0">Save Link</button>
                            <button onclick="autoFillSampleForm()" class="px-3 py-1.5 bg-slate-800 hover:bg-slate-900 text-white font-medium rounded-lg text-xs transition shrink-0">Fill Sample Candidate</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Form Card -->
            <form id="applicationForm" onsubmit="handleFormSubmit(event)" class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
                
                <div class="bg-slate-900 text-white p-6 sm:p-8 flex flex-col md:flex-row md:items-center justify-between gap-4">
                    <div>
                        <h2 class="text-xl sm:text-2xl font-bold">Candidate Intake & Pre-Interview Application</h2>
                        <p class="text-slate-400 text-xs sm:text-sm mt-1">Please complete all 5 sections prior to your scheduled interview.</p>
                    </div>
                    <div class="flex items-center space-x-2 bg-slate-800 p-2.5 rounded-xl border border-slate-700 text-xs text-slate-300 self-start md:self-auto">
                        <i class="fa-solid fa-lock text-brand-400 text-sm"></i>
                        <span>Secure Submission Portal</span>
                    </div>
                </div>

                <div class="p-6 sm:p-8 space-y-10">

                    <!-- SECTION 1: APPLICANT DETAILS -->
                    <section class="space-y-4">
                        <div class="flex items-center justify-between border-b border-slate-200 pb-3">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-full bg-brand-100 text-brand-700 font-bold text-xs flex items-center justify-center">1</span>
                                <h3 class="text-lg font-bold text-slate-900">Applicant Details</h3>
                            </div>
                            <span class="text-xs text-slate-400">* Required Fields</span>
                        </div>

                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Full Name *</label>
                                <input type="text" id="fullName" name="fullName" required placeholder="e.g. Jane Doe" class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500 outline-none">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Date of Birth *</label>
                                <input type="date" id="dob" name="dob" required class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500 outline-none">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Email Address *</label>
                                <input type="email" id="email" name="email" required placeholder="jane.doe@example.com" class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500 outline-none">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Phone Number *</label>
                                <input type="tel" id="phone" name="phone" required placeholder="(416) 555-0199" class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500 outline-none">
                            </div>

                            <div class="md:col-span-2">
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Residential Address *</label>
                                <input type="text" id="address" name="address" required placeholder="Street address, City, Province, Postal Code" class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500 outline-none">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Position Applied For *</label>
                                <input type="text" id="position" name="position" required placeholder="e.g. Residential Services Associate" class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500 outline-none">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Department / Location *</label>
                                <input type="text" id="department" name="department" required placeholder="e.g. Boarding Home Operations" class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500 outline-none">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Manager / Supervisor *</label>
                                <input type="text" id="manager" name="manager" required placeholder="Manager Name" class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500 outline-none">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Available Start Date *</label>
                                <input type="date" id="startDate" name="startDate" required class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500 outline-none">
                            </div>

                            <div class="md:col-span-2">
                                <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-2">Employment Type *</label>
                                <div class="grid grid-cols-1 sm:grid-cols-3 gap-3 bg-slate-50 p-3 rounded-xl border border-slate-200 text-sm">
                                    <label class="flex items-center space-x-2 cursor-pointer p-1.5 rounded hover:bg-white transition">
                                        <input type="radio" name="employmentType" value="Full-Time (FT)" required class="text-brand-600 focus:ring-brand-500">
                                        <span>Full-Time (FT)</span>
                                    </label>
                                    <label class="flex items-center space-x-2 cursor-pointer p-1.5 rounded hover:bg-white transition">
                                        <input type="radio" name="employmentType" value="Part-Time (PT)" class="text-brand-600 focus:ring-brand-500">
                                        <span>Part-Time (PT)</span>
                                    </label>
                                    <label class="flex items-center space-x-2 cursor-pointer p-1.5 rounded hover:bg-white transition">
                                        <input type="radio" name="employmentType" value="Independent Contractor" class="text-brand-600 focus:ring-brand-500">
                                        <span>Independent Contractor</span>
                                    </label>
                                </div>
                            </div>
                        </div>
                    </section>


                    <!-- SECTION 2: REFERENCES / EMERGENCY CONTACTS -->
                    <section class="space-y-4">
                        <div class="flex items-center space-x-3 border-b border-slate-200 pb-3">
                            <span class="w-7 h-7 rounded-full bg-brand-100 text-brand-700 font-bold text-xs flex items-center justify-center">2</span>
                            <h3 class="text-lg font-bold text-slate-900">Personal References & Emergency Contacts</h3>
                        </div>

                        <!-- Reference 1 -->
                        <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
                            <h4 class="text-xs font-bold text-slate-600 uppercase tracking-wider">Contact #1 (Primary Reference / Emergency Contact) *</h4>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                                <input type="text" id="ref1Name" name="ref1Name" placeholder="Full Name *" required class="px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                <input type="text" id="ref1Relation" name="ref1Relation" placeholder="Relation (e.g. Supervisor / Emergency) *" required class="px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                <input type="tel" id="ref1Phone" name="ref1Phone" placeholder="Phone Number *" required class="px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                <input type="email" id="ref1Email" name="ref1Email" placeholder="Email Address *" required class="px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                <input type="text" id="ref1Address" name="ref1Address" placeholder="Full Address *" required class="md:col-span-2 px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                            </div>
                        </div>

                        <!-- Reference 2 -->
                        <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
                            <h4 class="text-xs font-bold text-slate-600 uppercase tracking-wider">Contact #2 (Secondary Reference / Emergency Contact)</h4>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                                <input type="text" id="ref2Name" name="ref2Name" placeholder="Full Name" class="px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                <input type="text" id="ref2Relation" name="ref2Relation" placeholder="Relation" class="px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                <input type="tel" id="ref2Phone" name="ref2Phone" placeholder="Phone Number" class="px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                <input type="email" id="ref2Email" name="ref2Email" placeholder="Email Address" class="px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                <input type="text" id="ref2Address" name="ref2Address" placeholder="Full Address" class="md:col-span-2 px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                            </div>
                        </div>
                    </section>


                    <!-- SECTION 3: PREVIOUS EMPLOYMENT / CONTRACTS -->
                    <section class="space-y-4">
                        <div class="flex items-center space-x-3 border-b border-slate-200 pb-3">
                            <span class="w-7 h-7 rounded-full bg-brand-100 text-brand-700 font-bold text-xs flex items-center justify-center">3</span>
                            <h3 class="text-lg font-bold text-slate-900">Previous Employment & Service Contracts</h3>
                        </div>

                        <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                                <div>
                                    <label class="block text-xs font-semibold text-slate-600 mb-1">Position Held *</label>
                                    <input type="text" id="prevPosition" name="prevPosition" required placeholder="e.g. Cook / Residential Aide / Cleaner" class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                </div>
                                <div>
                                    <label class="block text-xs font-semibold text-slate-600 mb-1">Employer / Organization *</label>
                                    <input type="text" id="prevEmployer" name="prevEmployer" required placeholder="Care Facility / Organization Name" class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                </div>
                                <div>
                                    <label class="block text-xs font-semibold text-slate-600 mb-1">Supervisor Name *</label>
                                    <input type="text" id="prevSupervisor" name="prevSupervisor" required placeholder="Supervisor Full Name" class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                </div>
                                <div>
                                    <label class="block text-xs font-semibold text-slate-600 mb-1">Supervisor Phone *</label>
                                    <input type="tel" id="prevSupervisorPhone" name="prevSupervisorPhone" required placeholder="(416) 000-0000" class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                </div>
                                <div class="md:col-span-2">
                                    <label class="block text-xs font-semibold text-slate-600 mb-1">Key Responsibilities *</label>
                                    <textarea id="prevResponsibilities" name="prevResponsibilities" required rows="3" placeholder="Detail meal preparation, hygiene/sanitization, resident support..." class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none"></textarea>
                                </div>
                                <div class="md:col-span-2">
                                    <label class="block text-xs font-semibold text-slate-600 mb-1">Reason for Leaving *</label>
                                    <input type="text" id="prevReasonLeaving" name="prevReasonLeaving" required placeholder="e.g. Contract completed, schedule rotation change" class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none">
                                </div>
                            </div>
                        </div>
                    </section>


                    <!-- SECTION 4: EDUCATION / TRAINING -->
                    <section class="space-y-4">
                        <div class="flex items-center space-x-3 border-b border-slate-200 pb-3">
                            <span class="w-7 h-7 rounded-full bg-brand-100 text-brand-700 font-bold text-xs flex items-center justify-center">4</span>
                            <h3 class="text-lg font-bold text-slate-900">Education, Certifications & Resume</h3>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Diplomas, Degrees, Certifications & Licenses *</label>
                            <p class="text-xs text-slate-500 mb-2">Include Food Handler Certificate, First Aid/CPR, WHMIS, or medication handling training.</p>
                            <textarea id="educationTraining" name="educationTraining" required rows="3" placeholder="Certified Food Handler (Ontario), First Aid / CPR Level C, High School Diploma, WHMIS..." class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 outline-none"></textarea>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Attach Resume / CV (Optional)</label>
                            <p class="text-xs text-slate-500 mb-2">Upload your resume in PDF, Word (.doc, .docx), or TXT format (Max 5MB).</p>
                            
                            <div id="fileDropZone" class="border-2 border-dashed border-slate-300 hover:border-brand-500 rounded-xl p-5 text-center bg-slate-50 hover:bg-brand-50/30 transition cursor-pointer relative">
                                <input type="file" id="resumeFile" name="resumeFile" accept=".pdf,.doc,.docx,.txt,.rtf" onchange="handleFileSelect(event)" class="absolute inset-0 w-full h-full opacity-0 cursor-pointer">
                                <div id="uploadPlaceholder" class="space-y-1">
                                    <i class="fa-solid fa-cloud-arrow-up text-3xl text-brand-600 mb-1"></i>
                                    <p class="text-xs sm:text-sm font-semibold text-slate-700">Click or drag & drop to attach your resume</p>
                                    <p class="text-[11px] text-slate-400">Accepted formats: PDF, DOC, DOCX, TXT (Up to 5MB)</p>
                                </div>
                                <div id="fileDetails" class="hidden flex items-center justify-between bg-white p-3 rounded-lg border border-slate-200 shadow-sm text-left">
                                    <div class="flex items-center space-x-3 overflow-hidden">
                                        <div class="w-9 h-9 rounded-lg bg-brand-100 text-brand-700 flex items-center justify-center shrink-0 font-bold text-sm">
                                            <i class="fa-solid fa-file-lines"></i>
                                        </div>
                                        <div class="truncate">
                                            <p id="fileName" class="text-xs font-bold text-slate-800 truncate">resume.pdf</p>
                                            <p id="fileSize" class="text-[10px] text-slate-500">245 KB</p>
                                        </div>
                                    </div>
                                    <button type="button" onclick="removeSelectedFile(event)" class="text-slate-400 hover:text-rose-600 p-1.5 rounded-lg hover:bg-slate-100 transition">
                                        <i class="fa-solid fa-xmark text-base"></i>
                                    </button>
                                </div>
                            </div>
                            <input type="hidden" id="resumeBase64" name="resumeBase64">
                            <input type="hidden" id="resumeFileName" name="resumeFileName">
                        </div>
                    </section>


                    <!-- SECTION 5: RELEVANT EXPERIENCE & DECLARATION -->
                    <section class="space-y-4">
                        <div class="flex items-center space-x-3 border-b border-slate-200 pb-3">
                            <span class="w-7 h-7 rounded-full bg-brand-100 text-brand-700 font-bold text-xs flex items-center justify-center">5</span>
                            <h3 class="text-lg font-bold text-slate-900">Relevant Experience / Notes & Formal Declaration</h3>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Relevant Experience & Shift Rotation Notes *</label>
                            <textarea id="relevantNotes" name="relevantNotes" required rows="3" placeholder="Describe relevant experience in supportive living, meal preparation, or flexibility with cyclical shift schedules..." class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 outline-none"></textarea>
                        </div>

                        <!-- Formal Declaration Box -->
                        <div class="bg-slate-50 border border-slate-300 rounded-xl p-5 space-y-4">
                            <h4 class="text-xs font-bold text-slate-800 uppercase tracking-wider">Declaration Statement</h4>
                            
                            <div class="text-xs text-slate-700 leading-relaxed bg-white p-4 rounded-lg border border-slate-200 font-medium">
                                "I certify that the information provided is true and complete. I understand that submission of this application does not create an employment relationship. If engaged, I will provide services as an independent contractor and may be required to provide proof of insurance, licenses, certifications, and other documentation as requested."
                            </div>

                            <div class="space-y-3">
                                <label class="flex items-start space-x-3 cursor-pointer">
                                    <input type="checkbox" id="declarationCheck" name="declarationCheck" required class="mt-0.5 rounded text-brand-600 focus:ring-brand-500">
                                    <span class="text-xs text-slate-800 font-semibold">I have read and agree to the declaration statement above. *</span>
                                </label>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">Digital Signature (Type Full Legal Name) *</label>
                                    <input type="text" id="signatureName" name="signatureName" required placeholder="Type your full legal name" class="w-full px-3.5 py-2 border border-slate-300 rounded-lg text-sm bg-white focus:ring-2 focus:ring-brand-500 outline-none font-serif italic text-base text-brand-800">
                                    <p class="text-[11px] text-slate-500 mt-1">By checking the box above and typing your name, you are signing to the above declaration.</p>
                                </div>
                            </div>
                        </div>
                    </section>

                    <!-- Submit Button -->
                    <div class="pt-4 border-t border-slate-200 flex items-center justify-end">
                        <button type="submit" id="submitBtn" class="w-full sm:w-auto px-8 py-3 bg-brand-700 hover:bg-brand-800 text-white font-bold rounded-xl shadow-md hover:shadow-lg transition transform active:scale-95 flex items-center justify-center space-x-2">
                            <span>Submit Application</span>
                            <i class="fa-solid fa-paper-plane"></i>
                        </button>
                    </div>

                </div>
            </form>
        </div>


        <!-- TAB 2: GOOGLE SHEETS SETUP GUIDE -->
        <div id="tab-guide" class="hidden space-y-6">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 sm:p-8 space-y-6">
                <div>
                    <h2 class="text-xl font-bold text-slate-900">Google Drive & Spreadsheet Integration Guide</h2>
                    <p class="text-sm text-slate-500 mt-1">Follow these 4 steps to link this intake form directly to a Google Sheet on your Google Drive.</p>
                </div>

                <div class="space-y-6 text-sm text-slate-700">
                    
                    <!-- Step 1 -->
                    <div class="flex items-start space-x-4">
                        <div class="w-8 h-8 rounded-full bg-brand-700 text-white font-bold text-sm flex items-center justify-center shrink-0">1</div>
                        <div>
                            <h3 class="font-bold text-slate-900">Create a New Google Sheet</h3>
                            <p class="text-xs text-slate-600 mt-0.5">Go to your Google Drive, click <strong>+ New</strong> &gt; <strong>Google Sheets</strong>. Name it <em>"Candidate Intake Database"</em>.</p>
                        </div>
                    </div>

                    <!-- Step 2 -->
                    <div class="flex items-start space-x-4">
                        <div class="w-8 h-8 rounded-full bg-brand-700 text-white font-bold text-sm flex items-center justify-center shrink-0">2</div>
                        <div>
                            <h3 class="font-bold text-slate-900">Open Apps Script Editor</h3>
                            <p class="text-xs text-slate-600 mt-0.5">In your sheet menu, click <strong>Extensions</strong> &gt; <strong>Apps Script</strong>. Clear out any default code in `Code.gs`.</p>
                        </div>
                    </div>

                    <!-- Step 3 -->
                    <div class="flex items-start space-x-4">
                        <div class="w-8 h-8 rounded-full bg-brand-700 text-white font-bold text-sm flex items-center justify-center shrink-0">3</div>
                        <div class="flex-1">
                            <h3 class="font-bold text-slate-900">Paste the Webhook Receiver Code</h3>
                            <p class="text-xs text-slate-600 mt-0.5">Copy and paste the code snippet below into the editor:</p>

                            <div class="relative mt-3">
                        <pre class="bg-slate-900 text-slate-100 p-4 rounded-xl text-xs overflow-x-auto font-mono"><code>function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  
  // Auto-create column headers on first entry
  if (sheet.getLastRow() === 0) {
    sheet.appendRow([
      "Timestamp", "Full Name", "DoB", "Email", "Phone", "Address", "Position", 
      "Department", "Manager/Supervisor", "Start Date", "Employment Type",
      "Ref 1 Name", "Ref 1 Relation", "Ref 1 Phone", "Ref 1 Email", "Ref 1 Address",
      "Ref 2 Name", "Ref 2 Relation", "Ref 2 Phone", "Ref 2 Email", "Ref 2 Address",
      "Prev Position", "Prev Employer", "Prev Supervisor", "Prev Sup Phone", 
      "Responsibilities", "Reason Leaving", "Education & Training", "Resume Attached",
      "Relevant Experience", "Declaration Agreed", "Digital Signature"
    ]);
    sheet.getRange(1, 1, 1, 32).setFontWeight("bold").setBackground("#0284c7").setFontColor("#ffffff");
  }
  
  sheet.appendRow([
    new Date(), data.fullName, data.dob, data.email, data.phone, data.address, data.position,
    data.department, data.manager, data.startDate, data.employmentType,
    data.ref1Name, data.ref1Relation, data.ref1Phone, data.ref1Email, data.ref1Address,
    data.ref2Name, data.ref2Relation, data.ref2Phone, data.ref2Email, data.ref2Address,
    data.prevPosition, data.prevEmployer, data.prevSupervisor, data.prevSupervisorPhone,
    data.prevResponsibilities, data.prevReasonLeaving, data.educationTraining,
    data.resumeFileName ? data.resumeFileName : "None",
    data.relevantNotes, data.declarationCheck ? "YES" : "NO", data.signatureName
  ]);
  
  return ContentService.createTextOutput(JSON.stringify({ result: "success" }))
    .setMimeType(ContentService.MimeType.JSON);
}</code></pre>
                        <button onclick="copyScriptCode()" class="absolute top-3 right-3 px-3 py-1 bg-brand-600 hover:bg-brand-500 text-white rounded-lg text-xs font-semibold transition">
                            <i class="fa-regular fa-copy mr-1"></i> Copy Code
                        </button>
                    </div>
                        </div>
                    </div>

                    <!-- Step 4 -->
                    <div class="flex items-start space-x-4">
                        <div class="w-8 h-8 rounded-full bg-brand-700 text-white font-bold text-sm flex items-center justify-center shrink-0">4</div>
                        <div>
                            <h3 class="font-bold text-slate-900">Deploy as Web App & Connect URL</h3>
                            <ol class="list-decimal list-inside text-xs text-slate-600 mt-1 space-y-1">
                                <li>Click <strong>Deploy</strong> &gt; <strong>New Deployment</strong>.</li>
                                <li>Select type: <strong>Web App</strong>.</li>
                                <li>Set <em>Execute as:</em> <strong>Me</strong>.</li>
                                <li>Set <em>Who has access:</em> <strong>Anyone</strong>.</li>
                                <li>Click <strong>Deploy</strong>, authorize access, and copy the <strong>Web App URL</strong>.</li>
                                <li>Paste that Web App URL into the top banner input on the Form tab!</li>
                            </ol>
                        </div>
                    </div>

                </div>
            </div>
        </div>

    </main>

    <!-- Submission Success Modal -->
    <div id="successModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-md w-full p-6 space-y-4 shadow-2xl border border-slate-200 text-center">
            <div class="w-12 h-12 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center text-2xl mx-auto">
                <i class="fa-solid fa-check"></i>
            </div>
            <div>
                <h3 class="text-lg font-bold text-slate-900">Application Submitted!</h3>
                <p class="text-xs text-slate-600 mt-1">Thank you for submitting your details. Your candidate record has been registered for interview review.</p>
            </div>
            <div class="pt-2">
                <button onclick="closeModal()" class="w-full py-2.5 bg-brand-700 hover:bg-brand-800 text-white font-bold rounded-xl text-xs transition">
                    Done
                </button>
            </div>
        </div>
    </div>

    <script>
        let webhookUrl = localStorage.getItem(https://script.google.com/macros/s/AKfycbws-knZ8Lh3LJdRHxXGBhQLUMfsO--wSCxWYq-7cB2MH0o1qIeVaO75GyNm1htiWIc/exec) || '';

        document.addEventListener('DOMContentLoaded', () => {
            if (webhookUrl) {
                const input = document.getElementById('script-url-input');
                if(input) input.value = webhookUrl;
                updateBannerState();
            }
        });

        function handleFileSelect(event) {
            const file = event.target.files[0];
            if (!file) return;

            if (file.size > 5 * 1024 * 1024) {
                showToast("File size exceeds 5MB limit.", "error");
                event.target.value = '';
                return;
            }

            document.getElementById('uploadPlaceholder').classList.add('hidden');
            document.getElementById('fileDetails').classList.remove('hidden');
            document.getElementById('fileName').innerText = file.name;
            document.getElementById('fileSize').innerText = (file.size / 1024).toFixed(1) + ' KB';
            document.getElementById('resumeFileName').value = file.name;

            const reader = new FileReader();
            reader.onload = function(e) {
                document.getElementById('resumeBase64').value = e.target.result;
            };
            reader.readAsDataURL(file);
        }

        function removeSelectedFile(event) {
            event.stopPropagation();
            event.preventDefault();
            document.getElementById('resumeFile').value = '';
            document.getElementById('resumeBase64').value = '';
            document.getElementById('resumeFileName').value = '';
            document.getElementById('fileDetails').classList.add('hidden');
            document.getElementById('uploadPlaceholder').classList.remove('hidden');
        }

        function switchTab(tab) {
            const elForm = document.getElementById('tab-form');
            const elGuide = document.getElementById('tab-guide');
            const btnForm = document.getElementById('btn-tab-form');
            const btnGuide = document.getElementById('btn-tab-guide');

            if (tab === 'form') {
                elForm.classList.remove('hidden');
                elGuide.classList.add('hidden');
                btnForm.className = "px-3 py-2 rounded-lg bg-brand-600 text-white shadow-sm transition flex items-center space-x-1.5 font-semibold";
                btnGuide.className = "px-3 py-2 rounded-lg text-slate-300 hover:text-white hover:bg-slate-700 transition flex items-center space-x-1.5 font-normal";
            } else {
                elForm.classList.add('hidden');
                elGuide.classList.remove('hidden');
                btnGuide.className = "px-3 py-2 rounded-lg bg-brand-600 text-white shadow-sm transition flex items-center space-x-1.5 font-semibold";
                btnForm.className = "px-3 py-2 rounded-lg text-slate-300 hover:text-white hover:bg-slate-700 transition flex items-center space-x-1.5 font-normal";
            }
        }

        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            const bg = type === 'error' ? 'bg-rose-900 text-white' : type === 'success' ? 'bg-emerald-900 text-white' : 'bg-slate-900 text-white';
            toast.className = `${bg} px-4 py-2.5 rounded-xl shadow-xl text-xs font-medium flex items-center space-x-2 transition duration-300 pointer-events-auto transform translate-y-2 opacity-0`;
            toast.innerHTML = `<i class="fa-solid ${type === 'error' ? 'fa-triangle-exclamation text-rose-400' : 'fa-circle-info text-brand-400'}"></i><span>${message}</span>`;
            container.appendChild(toast);
            
            setTimeout(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            }, 10);

            setTimeout(() => {
                toast.classList.add('opacity-0');
                setTimeout(() => toast.remove(), 300);
            }, 4000);
        }

        function saveWebhookUrl() {
            const input = document.getElementById('script-url-input').value.trim();
            webhookUrl = input;
            localStorage.setItem('app_script_webhook', input);
            updateBannerState();
            showToast("Google Sheets Webhook URL saved successfully!", "success");
        }

        function updateBannerState() {
            const banner = document.getElementById('webhook-status-banner');
            const badge = document.getElementById('webhook-badge');
            if (webhookUrl && banner) {
                banner.className = "bg-emerald-50 border border-emerald-200 text-emerald-900 rounded-2xl p-4 text-xs sm:text-sm flex items-start space-x-3 shadow-sm";
                banner.querySelector('i').className = "fa-solid fa-circle-check text-emerald-600 text-xl mt-0.5 shrink-0";
                if(badge) {
                    badge.className = "text-[10px] font-semibold bg-emerald-200 text-emerald-900 px-2 py-0.5 rounded-md";
                    badge.innerText = "Connected";
                }
            }
        }

        function autoFillSampleForm() {
            document.getElementById('fullName').value = "Hazel Clarke";
            document.getElementById('dob').value = "1988-05-14";
            document.getElementById('email').value = "hazel.clarke@example.com";
            document.getElementById('phone').value = "(416) 555-0188";
            document.getElementById('address').value = "124 Parkdale Ave, Toronto, ON";
            document.getElementById('position').value = "Residential Services Associate";
            document.getElementById('department').value = "18 Maynard Ave";
            document.getElementById('manager').value = "Delicia Daniels";
            document.getElementById('startDate').value = "2026-09-15";
            
            const radios = document.getElementsByName('employmentType');
            radios[2].checked = true; // Independent Contractor

            document.getElementById('ref1Name').value = "Janet Brown";
            document.getElementById('ref1Relation').value = "Former Operations Supervisor";
            document.getElementById('ref1Phone').value = "(416) 555-0921";
            document.getElementById('ref1Email').value = "janet.b@example.com";
            document.getElementById('ref1Address').value = "88 Queen St W, Toronto, ON";

            document.getElementById('ref2Name').value = "Marcus Vance";
            document.getElementById('ref2Relation').value = "Care Coordinator";
            document.getElementById('ref2Phone').value = "(416) 555-3882";
            document.getElementById('ref2Email').value = "m.vance@example.com";
            document.getElementById('ref2Address').value = "12 King St E, Toronto, ON";

            document.getElementById('prevPosition').value = "Residential Care Associate & Cook";
            document.getElementById('prevEmployer').value = "Supportive Housing Community Services";
            document.getElementById('prevSupervisor').value = "Delicia Daniels";
            document.getElementById('prevSupervisorPhone').value = "(416) 555-4411";
            document.getElementById('prevResponsibilities').value = "Meal preparation and nutrition for 15+ residents, kitchen hygiene, daily bathroom sanitization, checking resident medication logs.";
            document.getElementById('prevReasonLeaving').value = "Contract term completed; seeking new boarding home assignment.";

            document.getElementById('educationTraining').value = "Certified Food Handler (City of Toronto), CPR & First Aid Level C, WHMIS Training.";
            document.getElementById('relevantNotes').value = "4+ years experience in supportive living. Experienced with non-traditional shift rotations (7-on / 7-off, alternating weekend coverage).";
            
            document.getElementById('declarationCheck').checked = true;
            document.getElementById('signatureName').value = "Hazel Clarke";

            // Populate sample file placeholder
            document.getElementById('uploadPlaceholder').classList.add('hidden');
            document.getElementById('fileDetails').classList.remove('hidden');
            document.getElementById('fileName').innerText = "Hazel_Clarke_Resume_2026.pdf";
            document.getElementById('fileSize').innerText = "184.2 KB";
            document.getElementById('resumeFileName').value = "Hazel_Clarke_Resume_2026.pdf";
            document.getElementById('resumeBase64').value = "data:application/pdf;base64,sample_resume_content";

            showToast("Sample candidate data populated!", "info");
        }

        async function handleFormSubmit(event) {
            event.preventDefault();
            const submitBtn = document.getElementById('submitBtn');
            submitBtn.disabled = true;
            submitBtn.innerHTML = `<i class="fa-solid fa-spinner fa-spin"></i> <span>Submitting...</span>`;

            const formData = new FormData(event.target);
            const data = Object.fromEntries(formData.entries());

            if (webhookUrl) {
                try {
                    await fetch(webhookUrl, {
                        method: 'POST',
                        mode: 'no-cors',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(data)
                    });
                    showToast("Application submitted & sent to Google Sheet!", "success");
                } catch (err) {
                    console.error('Sheet Webhook submission error:', err);
                    showToast("Form saved locally (webhook response error).", "info");
                }
            } else {
                showToast("Form submitted! (Connect Google Webhook URL in banner to stream to Drive)", "info");
            }

            document.getElementById('successModal').classList.remove('hidden');

            submitBtn.disabled = false;
            submitBtn.innerHTML = `<span>Submit Application</span> <i class="fa-solid fa-paper-plane"></i>`;
        }

        function closeModal() {
            document.getElementById('successModal').classList.add('hidden');
            document.getElementById('applicationForm').reset();
        }

        function copyScriptCode() {
            const code = `function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  
  if (sheet.getLastRow() === 0) {
    sheet.appendRow([
      "Timestamp", "Full Name", "DoB", "Email", "Phone", "Address", "Position", 
      "Department", "Manager/Supervisor", "Start Date", "Employment Type",
      "Ref 1 Name", "Ref 1 Relation", "Ref 1 Phone", "Ref 1 Email", "Ref 1 Address",
      "Ref 2 Name", "Ref 2 Relation", "Ref 2 Phone", "Ref 2 Email", "Ref 2 Address",
      "Prev Position", "Prev Employer", "Prev Supervisor", "Prev Sup Phone", 
      "Responsibilities", "Reason Leaving", "Education & Training", "Resume Attached",
      "Relevant Experience", "Declaration Agreed", "Digital Signature"
    ]);
    sheet.getRange(1, 1, 1, 32).setFontWeight("bold").setBackground("#0284c7").setFontColor("#ffffff");
  }
  
  sheet.appendRow([
    new Date(), data.fullName, data.dob, data.email, data.phone, data.address, data.position,
    data.department, data.manager, data.startDate, data.employmentType,
    data.ref1Name, data.ref1Relation, data.ref1Phone, data.ref1Email, data.ref1Address,
    data.ref2Name, data.ref2Relation, data.ref2Phone, data.ref2Email, data.ref2Address,
    data.prevPosition, data.prevEmployer, data.prevSupervisor, data.prevSupervisorPhone,
    data.prevResponsibilities, data.prevReasonLeaving, data.educationTraining,
    data.resumeFileName ? data.resumeFileName : "None",
    data.relevantNotes, data.declarationCheck ? "YES" : "NO", data.signatureName
  ]);
  
  return ContentService.createTextOutput(JSON.stringify({ result: "success" }))
    .setMimeType(ContentService.MimeType.JSON);
}`;
            navigator.clipboard.writeText(code);
            showToast("Google Apps Script code copied to clipboard!", "success");
        }
    </script>
</body>
</html>
