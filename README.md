<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Volopay AR Control Tower - E-Invoicing Prototype</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .step-active { border-color: #3B82F6; color: #3B82F6; }
    .status-badge { transition: all 0.3s ease; }
  </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen p-6 font-sans">

  <div class="max-w-6xl mx-auto space-y-8">
    
    <!-- HEADER -->
    <header class="flex justify-between items-center border-b border-slate-800 pb-4">
      <div>
        <h1 class="text-2xl font-bold text-blue-400">Volopay AR Control Tower</h1>
        <p class="text-xs text-slate-400 mt-1">E-Invoicing & Compliance Engine Prototype (India / Singapore)</p>
      </div>
      <div class="flex items-center space-x-3 text-xs">
        <span class="bg-emerald-900/50 text-emerald-400 border border-emerald-700 px-3 py-1 rounded-full flex items-center gap-1.5">
          <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span> Systems Operational
        </span>
      </div>
    </header>

    <!-- MAIN GRID -->
    <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
      
      <!-- LEFT COLUMN: INTERACTIVE FORM (7 COLS) -->
      <div class="lg:col-span-7 bg-slate-800 border border-slate-700 rounded-xl p-6 space-y-6 shadow-xl">
        <div class="flex justify-between items-center border-b border-slate-700 pb-3">
          <h2 class="text-lg font-semibold text-slate-200">Issue Compliance E-Invoice</h2>
          <span class="text-xs bg-slate-700 text-slate-300 px-2.5 py-1 rounded">Sandbox Mode</span>
        </div>

        <form id="invoiceForm" class="space-y-4">
          <!-- Target Region -->
          <div class="grid grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-medium text-slate-400 mb-1">Target Region & Network</label>
              <select id="region" class="w-full bg-slate-900 border border-slate-700 rounded p-2.5 text-sm text-slate-200 focus:border-blue-500 focus:outline-none" onchange="handleRegionChange()">
                <option value="IN">India (GST IRP Gateway)</option>
                <option value="SG">Singapore (IRAS InvoiceNow / Peppol)</option>
              </select>
            </div>
            <div>
              <label class="block text-xs font-medium text-slate-400 mb-1">Partner Access Point</label>
              <input type="text" id="partnerName" class="w-full bg-slate-900/60 border border-slate-800 rounded p-2.5 text-sm text-slate-400 cursor-not-allowed" value="ClearTax API (India)" readonly>
            </div>
          </div>

          <!-- Customer Details -->
          <div class="grid grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-medium text-slate-400 mb-1">Customer Name</label>
              <input type="text" id="customerName" value="Acme Logistics Pvt Ltd" class="w-full bg-slate-900 border border-slate-700 rounded p-2.5 text-sm text-slate-200 focus:border-blue-500 focus:outline-none">
            </div>
            <div>
              <label id="taxIdLabel" class="block text-xs font-medium text-slate-400 mb-1">GSTIN (15 Digits)</label>
              <input type="text" id="taxId" value="27AABCU9603R1ZM" class="w-full bg-slate-900 border border-slate-700 rounded p-2.5 text-sm text-slate-200 focus:border-blue-500 focus:outline-none font-mono">
            </div>
          </div>

          <!-- Invoice Item & Classification Code -->
          <div class="grid grid-cols-3 gap-4">
            <div class="col-span-2">
              <label class="block text-xs font-medium text-slate-400 mb-1">Item Description</label>
              <input type="text" id="itemDesc" value="Enterprise SaaS Subscription - Annual" class="w-full bg-slate-900 border border-slate-700 rounded p-2.5 text-sm text-slate-200 focus:border-blue-500 focus:outline-none">
            </div>
            <div>
              <label id="codeLabel" class="block text-xs font-medium text-slate-400 mb-1">HSN/SAC Code</label>
              <input type="text" id="taxCode" value="998313" class="w-full bg-slate-900 border border-slate-700 rounded p-2.5 text-sm text-slate-200 focus:border-blue-500 focus:outline-none font-mono">
            </div>
          </div>

          <!-- Value & Currency -->
          <div class="grid grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-medium text-slate-400 mb-1">Invoice Value (Net)</label>
              <input type="number" id="invoiceAmount" value="125000" class="w-full bg-slate-900 border border-slate-700 rounded p-2.5 text-sm text-slate-200 focus:border-blue-500 focus:outline-none font-mono">
            </div>
            <div>
              <label class="block text-xs font-medium text-slate-400 mb-1">Currency</label>
              <input type="text" id="currency" value="INR" class="w-full bg-slate-900/60 border border-slate-800 rounded p-2.5 text-sm text-slate-400 cursor-not-allowed" readonly>
            </div>
          </div>

          <!-- Action Buttons -->
          <div class="flex gap-3 pt-4">
            <button type="button" onclick="submitInvoice(false)" class="flex-1 bg-blue-600 hover:bg-blue-500 text-white font-medium py-2.5 px-4 rounded text-sm transition">
              Submit & Request IRN
            </button>
            <button type="button" onclick="submitInvoice(true)" class="bg-amber-600/20 hover:bg-amber-600/30 text-amber-300 border border-amber-700/50 font-medium py-2.5 px-4 rounded text-sm transition">
              Simulate API Error
            </button>
          </div>
        </form>

        <!-- LIVE LOG CONSOLE -->
        <div class="bg-slate-950 border border-slate-800 rounded-lg p-4 font-mono text-xs space-y-2">
          <div class="flex justify-between items-center text-slate-500 border-b border-slate-800 pb-2">
            <span>Execution Console Log</span>
            <button onclick="clearLogs()" class="hover:text-slate-300">Clear</button>
          </div>
          <div id="logConsole" class="h-32 overflow-y-auto space-y-1 text-slate-400">
            <div>[System]: Initialized Volopay Sandbox Middleware...</div>
          </div>
        </div>
      </div>

      <!-- RIGHT COLUMN: STATUS & ARCHITECTURE DIAGRAM (5 COLS) -->
      <div class="lg:col-span-5 space-y-6">
        
        <!-- STATUS RESULT CARD -->
        <div id="statusCard" class="bg-slate-800 border border-slate-700 rounded-xl p-6 space-y-4 shadow-xl">
          <h2 class="text-sm font-semibold text-slate-400 uppercase tracking-wider">Clearance Status</h2>
          
          <div id="idleState" class="text-center py-8 text-slate-500 space-y-2">
            <svg class="w-12 h-12 mx-auto text-slate-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>
            <p class="text-sm">Awaiting invoice submission...</p>
          </div>

          <div id="activeState" class="hidden space-y-4">
            <div class="flex items-center justify-between">
              <span id="badgeText" class="px-3 py-1 rounded-full text-xs font-semibold bg-blue-900/50 text-blue-400 border border-blue-700">Processing...</span>
              <span id="timestamp" class="text-xs text-slate-500">--</span>
            </div>

            <div class="space-y-2 text-xs font-mono bg-slate-900 p-3 rounded border border-slate-800">
              <div class="flex justify-between"><span class="text-slate-500">Invoice Ref:</span><span id="resRef" class="text-slate-200">--</span></div>
              <div class="flex justify-between"><span class="text-slate-500">Government IRN:</span><span id="resIrn" class="text-slate-200 truncate max-w-[200px]">--</span></div>
              <div class="flex justify-between"><span class="text-slate-500">Digital Signature:</span><span id="resSig" class="text-emerald-400">--</span></div>
            </div>

            <div id="errorBox" class="hidden bg-red-950/50 border border-red-800 text-red-300 p-3 rounded text-xs">
              <span class="font-bold block mb-1">Validation Error (ERR_TAX_GATEWAY_REJECT):</span>
              <span id="errorMessage">Invalid format encountered.</span>
            </div>
          </div>
        </div>

        <!-- ARCHITECTURE DIAGRAM SUMMARY -->
        <div class="bg-slate-800 border border-slate-700 rounded-xl p-6 space-y-4 shadow-xl">
          <h2 class="text-sm font-semibold text-slate-400 uppercase tracking-wider">System Architecture Topology</h2>
          
          <div class="space-y-3 text-xs font-mono">
            <div class="p-2.5 bg-slate-900 rounded border border-slate-700 flex items-center justify-between">
              <span class="text-blue-400 font-bold">[1] Volopay Web App</span>
              <span class="text-slate-500">Local Validation Layer</span>
            </div>
            <div class="text-center text-slate-600">↓ HTTPS / JSON Payload</div>
            <div class="p-2.5 bg-slate-900 rounded border border-slate-700 flex items-center justify-between">
              <span class="text-amber-400 font-bold">[2] Partner API Gateway</span>
              <span class="text-slate-500">ClearTax / Storecove</span>
            </div>
            <div class="text-center text-slate-600">↓ Schema / UBL 2.1 Format</div>
            <div class="p-2.5 bg-slate-900 rounded border border-slate-700 flex items-center justify-between">
              <span class="text-emerald-400 font-bold">[3] Government Node</span>
              <span class="text-slate-500">India IRP / Singapore IRAS</span>
            </div>
          </div>
        </div>

      </div>

    </div>
  </div>

  <script>
    function handleRegionChange() {
      const region = document.getElementById('region').value;
      const partner = document.getElementById('partnerName');
      const taxLabel = document.getElementById('taxIdLabel');
      const codeLabel = document.getElementById('codeLabel');
      const taxId = document.getElementById('taxId');
      const taxCode = document.getElementById('taxCode');
      const currency = document.getElementById('currency');

      if (region === 'IN') {
        partner.value = 'ClearTax API (India)';
        taxLabel.textContent = 'GSTIN (15 Digits)';
        codeLabel.textContent = 'HSN/SAC Code';
        taxId.value = '27AABCU9603R1ZM';
        taxCode.value = '998313';
        currency.value = 'INR';
      } else {
        partner.value = 'Storecove Access Point (Singapore)';
        taxLabel.textContent = 'UEN / Tax ID (9-10 Digits)';
        codeLabel.textContent = 'UNSPSC / Commodity Code';
        taxId.value = '201912345M';
        taxCode.value = '43231500';
        currency.value = 'SGD';
      }
      log(`Switched target region to ${region === 'IN' ? 'India (GST)' : 'Singapore (Peppol)'}`);
    }

    function log(msg) {
      const consoleEl = document.getElementById('logConsole');
      const time = new Date().toLocaleTimeString();
      const div = document.createElement('div');
      div.textContent = `[${time}] ${msg}`;
      consoleEl.appendChild(div);
      consoleEl.scrollTop = consoleEl.scrollHeight;
    }

    function clearLogs() {
      document.getElementById('logConsole').innerHTML = '';
    }

    function submitInvoice(simulateError) {
      const idleState = document.getElementById('idleState');
      const activeState = document.getElementById('activeState');
      const badge = document.getElementById('badgeText');
      const resRef = document.getElementById('resRef');
      const resIrn = document.getElementById('resIrn');
      const resSig = document.getElementById('resSig');
      const errorBox = document.getElementById('errorBox');
      const errorMessage = document.getElementById('errorMessage');

      idleState.classList.add('hidden');
      activeState.classList.remove('hidden');
      errorBox.classList.add('hidden');

      badge.className = "px-3 py-1 rounded-full text-xs font-semibold bg-blue-900/50 text-blue-400 border border-blue-700 animate-pulse";
      badge.textContent = "Validating Syntax...";
      
      const invNum = "INV-" + Math.floor(100000 + Math.random() * 900000);
      resRef.textContent = invNum;
      resIrn.textContent = "Processing...";
      resSig.textContent = "Pending...";
      document.getElementById('timestamp').textContent = new Date().toLocaleTimeString();

      log(`Initiating submission for ${invNum}...`);

      setTimeout(() => {
        log(`[Local Validation]: Tax ID & Code syntax check passed.`);
        badge.textContent = "Routing to Partner Gateway...";

        setTimeout(() => {
          if (simulateError) {
            badge.className = "px-3 py-1 rounded-full text-xs font-semibold bg-red-900/50 text-red-400 border border-red-700";
            badge.textContent = "Gateway Rejection";
            resIrn.textContent = "REJECTED";
            resSig.textContent = "NONE";
            errorMessage.textContent = "ERR_INVALID_HSN_CODE: HSN Code '998313' requires a 8-digit precision for cross-border transactions.";
            errorBox.classList.remove('hidden');
            log(`[API Error]: Gateway returned 400 Bad Request - ERR_INVALID_HSN_CODE.`);
          } else {
            badge.className = "px-3 py-1 rounded-full text-xs font-semibold bg-emerald-900/50 text-emerald-400 border border-emerald-700";
            badge.textContent = "IRN Cleared & Signed";
            const mockIrn = "irn_" + Array.from({length:24}, ()=>Math.floor(Math.random()*16).toString(16)).join('');
            resIrn.textContent = mockIrn;
            resSig.textContent = "VERIFIED (RSA-2048)";
            log(`[Success]: Received IRN from Tax Portal. Invoice cryptographically signed.`);
          }
        }, 1200);

      }, 800);
    }
  </script>

</body>
</html>
