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
              <input type="text" id="taxId" value="27AABCU9603R1ZM" class="w-full bg-slate-900 border border-
