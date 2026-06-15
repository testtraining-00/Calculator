<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Multi-Month Pitch Visualizer</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* Ensures the range sliders use your brand color */
    input[type=range] {
      accent-color: #ff5400;
    }
  </style>
</head>
<body class="min-h-screen bg-gray-50 flex items-center justify-center p-4 sm:p-8 font-sans text-gray-800">
  
  <div class="max-w-5xl w-full bg-white rounded-2xl shadow-xl overflow-hidden border-t-8" style="border-color: #ff5400;">
    
    <!-- Header Section -->
    <div class="p-6 sm:p-10 bg-white border-b border-gray-100">
      <div class="flex items-center gap-4 mb-4">
        <div class="p-3 rounded-xl shadow-sm" style="background-color: #fff0e6;">
          <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="#ff5400" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
            <polyline points="22 4 12 14.01 9 11.01"></polyline>
          </svg>
        </div>
        <h1 class="text-2xl sm:text-3xl font-bold text-gray-900">Pitching Multi-Month Upfront Payments</h1>
      </div>
      <p class="text-gray-600 text-lg leading-relaxed max-w-3xl">
        The most effective strategy when pitching multi-month payments is to compare the combined cost of the discounted device and the multi-month plan fee against the full Suggested Retail Price (SRP).
      </p>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-12 gap-0">
      
      <!-- Left Column: Interactive Calculator Controls -->
      <div class="lg:col-span-5 p-6 sm:p-10 bg-gray-50 border-r border-gray-100">
        <h2 class="text-xl font-bold mb-6 text-gray-800 flex items-center gap-2">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#ff5400" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <rect x="4" y="4" width="16" height="16" rx="2" ry="2"></rect>
            <rect x="9" y="9" width="6" height="6"></rect>
            <line x1="9" y1="1" x2="9" y2="4"></line>
            <line x1="15" y1="1" x2="15" y2="4"></line>
            <line x1="9" y1="20" x2="9" y2="23"></line>
            <line x1="15" y1="20" x2="15" y2="23"></line>
            <line x1="20" y1="9" x2="23" y2="9"></line>
            <line x1="20" y1="14" x2="23" y2="14"></line>
            <line x1="1" y1="9" x2="4" y2="9"></line>
            <line x1="1" y1="14" x2="4" y2="14"></line>
          </svg>
          Calculate the Value
        </h2>

        <!-- Transaction Type Toggle -->
        <div class="flex bg-gray-200 p-1 rounded-lg mb-6">
          <button id="btn-single" class="flex-1 bg-white shadow-sm py-2 rounded-md font-bold text-sm text-gray-800 transition-all" onclick="setMode('single')">New Account</button>
          <button id="btn-aal" class="flex-1 py-2 rounded-md font-bold text-sm text-gray-500 hover:text-gray-800 transition-all" onclick="setMode('aal')">Add a Line (AAL)</button>
        </div>
        
        <div class="space-y-6">
          <!-- Slider 1: Full SRP (Always Visible) -->
          <div class="space-y-2">
            <div class="flex justify-between items-center">
              <label class="text-sm font-semibold text-gray-700">Device Full SRP</label>
              <span class="font-bold" style="color: #ff5400;" id="srp-val">$600.00</span>
            </div>
            <input 
              type="range" min="100" max="1300" step="10" value="600" id="srp-input"
              class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
            />
          </div>

          <!-- Slider 2: Discounted Price (Always Visible) -->
          <div class="space-y-2">
            <div class="flex justify-between items-center">
              <label class="text-sm font-semibold text-gray-700">Discounted Device Price</label>
              <span class="font-bold" style="color: #ff5400;" id="discount-val">$300.00</span>
            </div>
            <input 
              type="range" min="0" max="600" step="10" value="300" id="discount-input"
              class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
            />
          </div>

          <!-- SINGLE LINE ONLY: Slider 3: Monthly Fee -->
          <div id="single-controls" class="space-y-2 block">
            <div class="flex justify-between items-center">
              <label class="text-sm font-semibold text-gray-700">Monthly Plan Fee</label>
              <span class="font-bold" style="color: #ff5400;" id="fee-val">$60.00</span>
            </div>
            <input 
              type="range" min="10" max="65" step="5" value="60" id="monthly-fee-input"
              class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
            />
          </div>

          <!-- AAL ONLY: Plan, Line, Days Left -->
          <div id="aal-controls" class="space-y-6 hidden">
            <!-- Select Base Plan -->
            <div class="space-y-2">
              <label class="text-sm font-semibold text-gray-700 block">New Line Plan</label>
              <select id="aal-plan" class="w-full p-2 border border-gray-300 rounded-lg text-sm bg-white focus:outline-none focus:ring-2 focus:ring-[#ff5400]" onchange="calculate()">
                <option value="50">$50 Unlimited+</option>
                <option value="60" selected>$60 Unlimited Premium</option>
              </select>
            </div>
            
            <!-- Select Line Number -->
            <div class="space-y-2">
              <label class="text-sm font-semibold text-gray-700 block">Which line is this?</label>
              <select id="aal-line" class="w-full p-2 border border-gray-300 rounded-lg text-sm bg-white focus:outline-none focus:ring-2 focus:ring-[#ff5400]" onchange="calculate()">
                <option value="2">Line 2</option>
                <option value="3">Line 3</option>
                <option value="4">Line 4</option>
                <option value="5">Line 5</option>
                <option value="6">Line 6</option>
                <option value="7">Line 7</option>
              </select>
            </div>

            <!-- Days Left in Cycle -->
            <div class="space-y-2">
              <div class="flex justify-between items-center">
                <label class="text-sm font-semibold text-gray-700">Days Left in Billing Cycle</label>
                <span class="font-bold" style="color: #ff5400;" id="days-val">15 Days</span>
              </div>
              <input 
                type="range" min="1" max="30" step="1" value="15" id="aal-days-input"
                class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
                oninput="calculate()"
              />
            </div>
          </div>

          <!-- Slider 4: Months (Always Visible) -->
          <div class="space-y-2">
            <div class="flex justify-between items-center">
              <label class="text-sm font-semibold text-gray-700">Months Upfront</label>
              <span class="font-bold" style="color: #ff5400;" id="months-val">2 Months</span>
            </div>
            <input 
              type="range" min="1" max="2" step="1" value="2" id="months-input"
              class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
            />
          </div>
        </div>
        
        <!-- Dynamic Pitch Tip -->
        <div class="mt-8 p-4 rounded-xl border" style="background-color: #fff0e6; border-color: #ffd9c2;">
          <h3 class="font-bold flex items-center gap-2 mb-2" style="color: #e64a00;">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M12 2v20M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"></path>
            </svg>
            The Pitch
          </h3>
          <p class="text-sm text-gray-800 leading-tight" id="pitch-text">
            <!-- Filled via JS -->
          </p>
        </div>
      </div>

      <!-- Right Column: Visual Comparison -->
      <div class="lg:col-span-7 p-6 sm:p-10">
        <h2 class="text-xl font-bold mb-6 text-gray-800">Cost Comparison</h2>
        
        <div class="space-y-6">
          
          <!-- Option 1: Standard Option -->
          <div class="bg-white border-2 border-gray-100 rounded-2xl p-5 relative overflow-hidden">
            <div class="flex justify-between items-start mb-4">
              <div>
                <h3 class="text-lg font-bold text-gray-500 uppercase tracking-wide text-sm">Standard Option</h3>
                <p class="text-gray-400 text-xs mt-1">No Offer Applied</p>
              </div>
              <div class="text-right">
                <span class="text-2xl font-black text-gray-400" id="std-total"></span>
                <p class="text-gray-400 text-xs font-semibold">Total Initial Cost</p>
              </div>
            </div>
            
            <div class="space-y-3 relative z-10">
              <div class="flex justify-between items-center text-sm">
                <span class="flex items-center gap-2 text-gray-600">
                  <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="5" y="2" width="14" height="20" rx="2" ry="2"></rect><line x1="12" y1="18" x2="12.01" y2="18"></line></svg>
                  Device Price (Full SRP)
                </span>
                <span class="font-semibold std-srp"></span>
              </div>
              <div class="flex justify-between items-center text-sm">
                <span class="flex items-center gap-2 text-gray-600" id="std-plan-label">
                  <!-- Filled via JS -->
                </span>
                <span class="font-semibold std-fee"></span>
              </div>
            </div>
          </div>

          <!-- VS Divider -->
          <div class="flex items-center justify-center -my-2 relative z-10">
            <span class="bg-gray-100 text-gray-400 text-xs font-bold px-3 py-1 rounded-full uppercase tracking-widest border-2 border-white shadow-sm">VS</span>
          </div>

          <!-- Option 2: Multi-Month Payment -->
          <div class="bg-white border-2 rounded-2xl p-5 relative overflow-hidden shadow-lg transform transition-all hover:scale-[1.01]" style="border-color: #ff5400;">
            <div class="absolute top-0 right-0 w-32 h-32 bg-opacity-10 rounded-bl-full -mr-10 -mt-10" style="background-color: #ff5400;"></div>
            
            <div class="flex justify-between items-start mb-4 relative z-10">
              <div>
                <h3 class="text-lg font-black uppercase tracking-wide" style="color: #ff5400;">Multi-Month Payment</h3>
                <div class="inline-block mt-1 px-2 py-0.5 rounded text-xs font-bold text-white" style="background-color: #ff5400;">Recommended Pitch</div>
              </div>
              <div class="text-right">
                <span class="text-3xl font-black text-gray-900" id="mm-total"></span>
                <p class="text-gray-500 text-xs font-semibold">Total Upfront Cost</p>
              </div>
            </div>
            
            <!-- Dynamic Breakdown Section -->
            <div id="mm-breakdown" class="space-y-3 relative z-10">
               <!-- Filled via JS -->
            </div>

            <!-- AAL Future Credit Banner -->
            <div id="aal-credit-banner" class="hidden mt-4 p-3 rounded-lg border-2 border-dashed relative z-10" style="background-color: #fff9f5; border-color: #ffcba6;">
              <p class="text-xs font-semibold text-gray-800" id="aal-credit-text"></p>
            </div>

            <!-- Visual Bar Comparison against SRP -->
            <div class="pt-4 mt-4 border-t border-gray-100 relative z-10">
              <p class="text-xs text-gray-500 mb-2 font-medium">Comparison against Full Device SRP:</p>
              
              <!-- SRP Bar -->
              <div class="flex items-center gap-3 mb-2">
                <div class="w-20 text-xs font-semibold text-gray-400 text-right">Device SRP</div>
                <div class="flex-1 h-3 bg-gray-100 rounded-full overflow-hidden">
                  <div class="h-full bg-gray-400 rounded-full" style="width: 100%;"></div>
                </div>
                <div class="w-16 text-xs font-bold text-gray-400 pitch-srp"></div>
              </div>

              <!-- Multi-Month Total Bar -->
              <div class="flex items-center gap-3">
                <div class="w-20 text-xs font-bold text-gray-800 text-right">Bundle Total</div>
                <div class="flex-1 h-3 bg-gray-100 rounded-full overflow-hidden relative">
                  <div id="bundle-bar-fill" class="h-full rounded-full transition-all duration-300" style="background-color: #ff5400;"></div>
                </div>
                <div class="w-16 text-xs font-black" style="color: #ff5400;" id="bar-mm-text"></div>
              </div>
            </div>

          </div>

        </div>
      </div>
    </div>
  </div>

  <script>
    const formatCurrency = (val) => new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(val);

    // Multi-Line Discounts Data Map
    const planDiscounts = {
      50: { 2: 30, 3: 30, 4: 10, 5: 30, 6: 30, 7: 30 },
      60: { 2: 40, 3: 40, 4: 20, 5: 40, 6: 40, 7: 40 }
    };

    let mode = 'single'; // 'single' or 'aal'

    const srpInput = document.getElementById('srp-input');
    const discountInput = document.getElementById('discount-input');
    const monthlyFeeInput = document.getElementById('monthly-fee-input');
    const monthsInput = document.getElementById('months-input');
    const aalDaysInput = document.getElementById('aal-days-input');
    const aalPlanSelect = document.getElementById('aal-plan');
    const aalLineSelect = document.getElementById('aal-line');

    function setMode(newMode) {
      mode = newMode;
      
      const btnSingle = document.getElementById('btn-single');
      const btnAal = document.getElementById('btn-aal');
      const singleControls = document.getElementById('single-controls');
      const aalControls = document.getElementById('aal-controls');

      if (mode === 'single') {
        btnSingle.classList.add('bg-white', 'shadow-sm', 'text-gray-800');
        btnSingle.classList.remove('text-gray-500');
        btnAal.classList.add('text-gray-500');
        btnAal.classList.remove('bg-white', 'shadow-sm', 'text-gray-800');
        
        singleControls.classList.remove('hidden');
        singleControls.classList.add('block');
        aalControls.classList.add('hidden');
        aalControls.classList.remove('block');
      } else {
        btnAal.classList.add('bg-white', 'shadow-sm', 'text-gray-800');
        btnAal.classList.remove('text-gray-500');
        btnSingle.classList.add('text-gray-500');
        btnSingle.classList.remove('bg-white', 'shadow-sm', 'text-gray-800');
        
        singleControls.classList.add('hidden');
        singleControls.classList.remove('block');
        aalControls.classList.remove('hidden');
        aalControls.classList.add('block');
      }
      calculate();
    }

    function calculate() {
      const srp = parseFloat(srpInput.value);
      
      // Keep discount max linked to current SRP
      discountInput.max = srp;
      if (parseFloat(discountInput.value) > srp) {
        discountInput.value = srp;
      }
      
      const discount = parseFloat(discountInput.value);
      const months = parseFloat(monthsInput.value);

      let standardTotal, multiMonthTotal;
      let standardFeeLabel, standardFeeAmount;

      // Update Shared UI Elements
      document.getElementById('srp-val').innerText = formatCurrency(srp);
      document.getElementById('discount-val').innerText = formatCurrency(discount);
      document.getElementById('months-val').innerText = `${months} Months`;
      document.querySelectorAll('.pitch-srp').forEach(el => el.innerText = formatCurrency(srp));

      const svgPhone = `<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#ff5400" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="5" y="2" width="14" height="20" rx="2" ry="2"></rect><line x1="12" y1="18" x2="12.01" y2="18"></line></svg>`;
      const svgPlan = `<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#ff5400" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>`;

      if (mode === 'single') {
        // --- SINGLE LINE LOGIC ---
        const monthlyFee = parseFloat(monthlyFeeInput.value);
        document.getElementById('fee-val').innerText = formatCurrency(monthlyFee);
        
        standardTotal = srp + monthlyFee;
        multiMonthTotal = discount + (monthlyFee * months);
        const multiMonthPlanTotal = monthlyFee * months;

        standardFeeLabel = "Service Plan (1-month fee)";
        standardFeeAmount = formatCurrency(monthlyFee);

        // Pitch
        document.getElementById('pitch-text').innerHTML = `"Instead of paying <span class="pitch-srp font-semibold text-[#ff5400]">${formatCurrency(srp)}</span> just for the phone, you can get the phone <strong>AND ${months} months of service</strong> for <strong>${formatCurrency(multiMonthTotal)}</strong>!"`;

        // Breakdown Builder
        document.getElementById('mm-breakdown').innerHTML = `
          <div class="flex justify-between items-center text-sm">
            <span class="flex items-center gap-2 font-bold text-gray-800">${svgPhone} Discounted Device Price</span>
            <span class="font-bold text-lg" style="color: #ff5400;">${formatCurrency(discount)}</span>
          </div>
          <div class="flex justify-between items-center text-sm">
            <span class="flex items-center gap-2 font-bold text-gray-800">${svgPlan} ${months}-Month Upfront Plan Total</span>
            <span class="font-bold text-lg" style="color: #ff5400;">${formatCurrency(multiMonthPlanTotal)}</span>
          </div>
        `;
        document.getElementById('aal-credit-banner').classList.add('hidden');

      } else {
        // --- ADD A LINE LOGIC ---
        const basePlan = parseFloat(aalPlanSelect.value);
        const lineNum = parseInt(aalLineSelect.value);
        const daysLeft = parseInt(aalDaysInput.value);
        
        document.getElementById('days-val').innerText = `${daysLeft} Days`;

        const discountedRate = planDiscounts[basePlan][lineNum];
        // Calculate Proration
        const proratedFirstMonth = (discountedRate / 30) * daysLeft;
        // Full rate for multi-month upfront
        const prepayment = basePlan * months;

        standardTotal = srp + proratedFirstMonth;
        multiMonthTotal = discount + proratedFirstMonth + prepayment;

        standardFeeLabel = "Prorated First Month (Discounted Rate)";
        standardFeeAmount = formatCurrency(proratedFirstMonth);

        // Pitch
        document.getElementById('pitch-text').innerHTML = `"Instead of paying <span class="pitch-srp font-semibold text-[#ff5400]">${formatCurrency(srp)}</span> just for the phone, you can get the phone, your prorated first month, <strong>AND prepay your next ${months} months</strong> for <strong>${formatCurrency(multiMonthTotal)}</strong>!"`;

        // Breakdown Builder
        document.getElementById('mm-breakdown').innerHTML = `
          <div class="flex justify-between items-center text-sm">
            <span class="flex items-center gap-2 font-bold text-gray-800">${svgPhone} Discounted Device Price</span>
            <span class="font-bold text-lg" style="color: #ff5400;">${formatCurrency(discount)}</span>
          </div>
          <div class="flex justify-between items-center text-sm">
            <span class="flex items-center gap-2 font-bold text-gray-800">${svgPlan} Prorated First Month</span>
            <span class="font-bold text-lg" style="color: #ff5400;">${formatCurrency(proratedFirstMonth)}</span>
          </div>
          <div class="flex justify-between items-center text-sm">
            <span class="flex items-center gap-2 font-bold text-gray-800">${svgPlan} ${months}-Month Prepayment (Standard Rate)</span>
            <span class="font-bold text-lg" style="color: #ff5400;">${formatCurrency(prepayment)}</span>
          </div>
        `;

        // Credit Banner
        const creditBanner = document.getElementById('aal-credit-banner');
        creditBanner.classList.remove('hidden');
        document.getElementById('aal-credit-text').innerHTML = `<strong>Future Bill Impact:</strong> The account will receive a <strong>${formatCurrency(basePlan)} credit</strong> on the MRC for the next ${months} month(s) to offset this prepayment.`;
      }

      // Update Standard Option Details
      document.getElementById('std-total').innerText = formatCurrency(standardTotal);
      document.querySelectorAll('.std-srp').forEach(el => el.innerText = formatCurrency(srp));
      
      document.getElementById('std-plan-label').innerHTML = `
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>
        ${standardFeeLabel}
      `;
      document.querySelectorAll('.std-fee').forEach(el => el.innerText = standardFeeAmount);

      // Update Multi-Month Option Totals
      document.getElementById('mm-total').innerText = formatCurrency(multiMonthTotal);

      // Comparison Bar Graph Update
      document.getElementById('bar-mm-text').innerText = formatCurrency(multiMonthTotal);
      document.getElementById('bundle-bar-fill').style.width = `${Math.min((multiMonthTotal / srp) * 100, 100)}%`;
    }

    // Attach listeners
    srpInput.addEventListener('input', calculate);
    discountInput.addEventListener('input', calculate);
    monthlyFeeInput.addEventListener('input', calculate);
    monthsInput.addEventListener('input', calculate);

    // Initial calculation on load
    calculate();
  </script>
</body>
</html>
