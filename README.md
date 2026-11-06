<!DOCTYPE html>
<html lang="bn" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>EarnPay BD - মোবাইল ইনকাম অ্যাপ</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        bkash: '#e2136e',
                        nagad: '#f7931e',
                        rocket: '#8c3494',
                        upay: '#005baa'
                    },
                    fontFamily: {
                        sans: ['Hind Siliguri', 'Inter', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts for Bengali -->
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- FontAwesome Mobile Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Monetag Ad SDK -->
    <script src="//libtl.com/sdk.js" data-zone="12198706" data-sdk="show_12198706"></script>

    <style>
        body {
            font-family: 'Hind Siliguri', sans-serif;
            -webkit-tap-highlight-color: transparent;
            user-select: none;
        }
        /* App Tab Transitions */
        .tab-content {
            display: none;
            opacity: 0;
            transform: translateY(4px);
            transition: opacity 0.2s ease, transform 0.2s ease;
        }
        .tab-content.active {
            display: block;
            opacity: 1;
            transform: translateY(0);
        }
        /* Hide scrollbars for full native feel */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col justify-between overflow-x-hidden">

    <!-- 100% Full Width & Height App Header -->
    <header class="w-full bg-slate-900/95 backdrop-blur-md px-4 py-3 border-b border-slate-800/80 sticky top-0 z-40 flex items-center justify-between shadow-md">
        <div class="flex items-center gap-2.5">
            <div class="w-9 h-9 rounded-full bg-gradient-to-tr from-emerald-500 to-teal-400 flex items-center justify-center shadow-lg shadow-emerald-500/20">
                <i class="fa-solid fa-coins text-slate-950 text-base font-extrabold"></i>
            </div>
            <div>
                <h1 class="text-base font-bold text-white leading-tight flex items-center gap-1.5">
                    EarnPay <span class="bg-emerald-500/20 text-emerald-400 text-[10px] px-1.5 py-0.5 rounded-md font-normal border border-emerald-500/30">PRO</span>
                </h1>
                <p class="text-[11px] text-slate-400">প্রতি অ্যাড ৳২.০০ ইনকাম</p>
            </div>
        </div>

        <!-- Top Balance Badge -->
        <div class="bg-slate-800/90 border border-slate-700/70 px-3.5 py-1 rounded-full flex items-center gap-1.5 shadow-inner">
            <div class="w-5 h-5 rounded-full bg-amber-500/20 text-amber-400 flex items-center justify-center text-[11px] font-bold">৳</div>
            <span id="appHeaderBalance" class="text-sm font-bold text-emerald-400">৳0.00</span>
        </div>
    </header>

    <!-- Main Content Area (Full Screen Responsive Scrollable Container) -->
    <main class="w-full flex-grow overflow-y-auto px-4 py-4 space-y-4 no-scrollbar pb-24">

        <!-- TAB 1: HOME & AD WATCHING -->
        <div id="tab-home" class="tab-content active space-y-4">
            
            <!-- Full Width Hero Ad Card -->
            <div class="relative overflow-hidden rounded-2xl bg-gradient-to-b from-slate-800 to-slate-800/95 border-2 border-emerald-500/50 p-5 shadow-xl shadow-emerald-950/30 text-center space-y-4">
                <div class="absolute -top-10 -right-10 w-28 h-28 bg-emerald-500/10 rounded-full blur-2xl"></div>

                <div class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full bg-emerald-500/10 border border-emerald-500/30 text-emerald-400 text-xs font-semibold">
                    <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                    ইনস্ট্যান্ট রিওয়ার্ড অফার
                </div>

                <div class="space-y-1">
                    <h2 class="text-2xl font-black text-white">অ্যাড দেখুন ও আয় করুন</h2>
                    <p class="text-xs text-slate-300">একটি অ্যাড সম্পূর্ণ দেখলে ১০ সেকেন্ড কাউন্টডাউন শেষে সরাসরি আপনার একাউন্টে ৳২ যোগ হবে।</p>
                </div>

                <!-- Watch Ad Button -->
                <div class="space-y-3 pt-1">
                    <button id="watchAdBtn" onclick="handleWatchAd()" class="w-full py-3.5 px-4 rounded-xl bg-gradient-to-r from-emerald-500 to-teal-400 hover:from-emerald-400 hover:to-teal-300 text-slate-950 font-bold text-base shadow-lg shadow-emerald-500/25 active:scale-95 transition-all flex items-center justify-center gap-2">
                        <i class="fa-solid fa-circle-play text-xl"></i>
                        <span id="adBtnText">অ্যাড দেখুন (৳২.০০)</span>
                    </button>

                    <!-- Dynamic Timer & Progress -->
                    <div id="timerContainer" class="hidden bg-slate-900 border border-slate-700/80 p-3 rounded-xl space-y-2">
                        <div class="flex justify-between items-center text-xs">
                            <span class="text-slate-400">অ্যাড দেখা যাচাই হচ্ছে...</span>
                            <span id="timerText" class="font-extrabold text-emerald-400 text-sm">10s</span>
                        </div>
                        <div class="w-full bg-slate-800 h-2.5 rounded-full overflow-hidden">
                            <div id="progressBar" class="bg-emerald-500 h-full w-full transition-all duration-1000 ease-linear"></div>
                        </div>
                    </div>
                </div>

                <div class="text-[10px] text-slate-400 flex items-center justify-center gap-1 pt-1">
                    <i class="fa-solid fa-shield-halved text-emerald-400"></i> Monetag Verified Ad Zone
                </div>
            </div>

            <!-- Responsive Dashboard Cards Grid -->
            <div class="grid grid-cols-2 gap-3">
                <div class="bg-slate-800/80 border border-slate-700/60 p-3.5 rounded-2xl">
                    <div class="flex items-center justify-between text-slate-400 text-xs mb-1">
                        <span>বর্তমান ব্যালেন্স</span>
                        <i class="fa-solid fa-wallet text-emerald-400"></i>
                    </div>
                    <h3 id="dashBalance" class="text-xl font-bold text-emerald-400">৳0.00</h3>
                    <span class="text-[10px] text-slate-400">উইথড্র যোগ্য</span>
                </div>

                <div class="bg-slate-800/80 border border-slate-700/60 p-3.5 rounded-2xl">
                    <div class="flex items-center justify-between text-slate-400 text-xs mb-1">
                        <span>মোট অ্যাড দেখা</span>
                        <i class="fa-solid fa-eye text-blue-400"></i>
                    </div>
                    <h3 id="dashAdsCount" class="text-xl font-bold text-white">0 টি</h3>
                    <span class="text-[10px] text-slate-400">সফল ক্লিক</span>
                </div>

                <div class="bg-slate-800/80 border border-slate-700/60 p-3.5 rounded-2xl">
                    <div class="flex items-center justify-between text-slate-400 text-xs mb-1">
                        <span>মোট অর্জিত টাকা</span>
                        <i class="fa-solid fa-chart-line text-amber-400"></i>
                    </div>
                    <h3 id="dashTotalEarned" class="text-xl font-bold text-amber-400">৳0.00</h3>
                    <span class="text-[10px] text-slate-400">লাইফটাইম ইনকাম</span>
                </div>

                <div class="bg-slate-800/80 border border-slate-700/60 p-3.5 rounded-2xl">
                    <div class="flex items-center justify-between text-slate-400 text-xs mb-1">
                        <span>মোট উত্তোলন</span>
                        <i class="fa-solid fa-hand-holding-dollar text-purple-400"></i>
                    </div>
                    <h3 id="dashTotalWithdrawn" class="text-xl font-bold text-purple-400">৳0.00</h3>
                    <span class="text-[10px] text-slate-400">উইথড্রকৃত টাকা</span>
                </div>
            </div>

            <!-- Rules & Instructions -->
            <div class="bg-slate-800/40 border border-slate-800 p-3.5 rounded-2xl space-y-2">
                <h4 class="text-xs font-bold text-slate-200 flex items-center gap-1.5">
                    <i class="fa-solid fa-circle-question text-emerald-400"></i> কিভাবে কাজ করবেন?
                </h4>
                <ul class="text-[11px] text-slate-400 space-y-1 list-disc pl-4">
                    <li>উপরে "অ্যাড দেখুন" বাটনে ক্লিক করুন।</li>
                    <li>অ্যাড শো হলে সম্পূর্ণ দেখে স্কিপ বা ক্লোজ করুন।</li>
                    <li>১০ সেকেন্ড টাইমার শেষ হওয়া পর্যন্ত অপেক্ষা করুন।</li>
                    <li>ব্যালেন্স ৳১৫০ টাকা হলেই বিকাশ বা নগদে তুলে নিন।</li>
                </ul>
            </div>
        </div>

        <!-- TAB 2: WITHDRAW -->
        <div id="tab-withdraw" class="tab-content space-y-4">
            <div class="bg-slate-800/80 border border-slate-700/70 rounded-2xl p-4 space-y-4">
                <div class="border-b border-slate-700/60 pb-3">
                    <h3 class="text-base font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-money-bill-wave text-emerald-400"></i> টাকা উত্তোলন
                    </h3>
                    <p class="text-xs text-slate-400">নূন্যতম উইথড্র সীমা: ৳১৫০ টাকা</p>
                </div>

                <form id="mobileWithdrawForm" onsubmit="handleWithdraw(event)" class="space-y-4">
                    <!-- Method Selection Grid -->
                    <div>
                        <label class="block text-xs font-medium text-slate-300 mb-2">পেমেন্ট মেথড:</label>
                        <div class="grid grid-cols-2 gap-2.5">
                            <label class="flex items-center gap-2.5 p-3 rounded-xl border border-slate-700 bg-slate-900/60 cursor-pointer has-[:checked]:border-bkash has-[:checked]:bg-bkash/10">
                                <input type="radio" name="paymentMethod" value="bKash" checked class="sr-only">
                                <div class="w-7 h-7 rounded-full bg-bkash/20 text-bkash flex items-center justify-center font-bold text-xs">বিকাশ</div>
                                <span class="text-xs font-semibold text-slate-200">bKash</span>
                            </label>

                            <label class="flex items-center gap-2.5 p-3 rounded-xl border border-slate-700 bg-slate-900/60 cursor-pointer has-[:checked]:border-nagad has-[:checked]:bg-nagad/10">
                                <input type="radio" name="paymentMethod" value="Nagad" class="sr-only">
                                <div class="w-7 h-7 rounded-full bg-nagad/20 text-nagad flex items-center justify-center font-bold text-xs">নগদ</div>
                                <span class="text-xs font-semibold text-slate-200">Nagad</span>
                            </label>

                            <label class="flex items-center gap-2.5 p-3 rounded-xl border border-slate-700 bg-slate-900/60 cursor-pointer has-[:checked]:border-rocket has-[:checked]:bg-rocket/10">
                                <input type="radio" name="paymentMethod" value="Rocket" class="sr-only">
                                <div class="w-7 h-7 rounded-full bg-rocket/20 text-rocket flex items-center justify-center font-bold text-xs">রকেট</div>
                                <span class="text-xs font-semibold text-slate-200">Rocket</span>
                            </label>

                            <label class="flex items-center gap-2.5 p-3 rounded-xl border border-slate-700 bg-slate-900/60 cursor-pointer has-[:checked]:border-upay has-[:checked]:bg-upay/10">
                                <input type="radio" name="paymentMethod" value="Upay" class="sr-only">
                                <div class="w-7 h-7 rounded-full bg-upay/20 text-upay flex items-center justify-center font-bold text-xs">উপায়</div>
                                <span class="text-xs font-semibold text-slate-200">Upay</span>
                            </label>
                        </div>
                    </div>

                    <!-- Mobile Number -->
                    <div>
                        <label for="accNumber" class="block text-xs font-medium text-slate-300 mb-1">মোবাইল নম্বর</label>
                        <input type="tel" id="accNumber" placeholder="017XXXXXXXX" pattern="[0-9]{11}" required
                               class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm text-white focus:outline-none focus:border-emerald-500">
                    </div>

                    <!-- Amount -->
                    <div>
                        <label for="withdrawAmount" class="block text-xs font-medium text-slate-300 mb-1">উইথড্র পরিমাণ (৳)</label>
                        <input type="number" id="withdrawAmount" placeholder="১৫০ বা তার বেশি" min="150" required
                               class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm text-white focus:outline-none focus:border-emerald-500">
                    </div>

                    <button type="submit" class="w-full py-3 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold rounded-xl transition-all shadow-lg shadow-emerald-500/20 active:scale-95">
                        উইথড্র রিকোয়েস্ট পাঠান
                    </button>
                </form>
            </div>
        </div>

        <!-- TAB 3: HISTORY -->
        <div id="tab-history" class="tab-content space-y-3">
            <div class="flex items-center justify-between">
                <h3 class="text-sm font-bold text-white">উইথড্র রেকর্ডসমূহ</h3>
                <button onclick="clearHistory()" class="text-xs text-rose-400 hover:underline">ক্লিয়ার করুন</button>
            </div>

            <div id="historyList" class="space-y-2">
                <!-- Dynamic Withdrawal Items -->
            </div>
        </div>
    </main>

    <!-- Toast Floating Banner -->
    <div id="appToast" class="fixed bottom-20 left-1/2 -translate-x-1/2 w-[90%] max-w-xs z-50 pointer-events-none opacity-0 translate-y-4 transition-all duration-300">
        <div id="toastContent" class="bg-slate-800 border border-slate-700 px-4 py-3 rounded-xl shadow-2xl flex items-center gap-3 text-xs text-white">
            <i id="toastIcon" class="fa-solid fa-circle-info text-emerald-400 text-base"></i>
            <span id="toastMsg">মেসেজ টেক্সট</span>
        </div>
    </div>

    <!-- Mobile Bottom Navigation Bar (100% Full Width Fixed Nav) -->
    <nav class="fixed bottom-0 left-0 right-0 w-full bg-slate-900/95 backdrop-blur-lg border-t border-slate-800 px-8 py-2.5 z-40 flex items-center justify-between shadow-2xl">
        <button onclick="switchTab('home')" id="nav-home" class="flex flex-col items-center gap-1 text-emerald-400 transition-colors">
            <i class="fa-solid fa-house text-lg"></i>
            <span class="text-[10px] font-semibold">হোম</span>
        </button>

        <button onclick="switchTab('withdraw')" id="nav-withdraw" class="flex flex-col items-center gap-1 text-slate-400 transition-colors">
            <i class="fa-solid fa-wallet text-lg"></i>
            <span class="text-[10px] font-semibold">উইথড্র</span>
        </button>

        <button onclick="switchTab('history')" id="nav-history" class="flex flex-col items-center gap-1 text-slate-400 transition-colors">
            <i class="fa-solid fa-clock-rotate-left text-lg"></i>
            <span class="text-[10px] font-semibold">হিস্ট্রি</span>
        </button>
    </nav>

    <!-- Mobile App Logic Script -->
    <script>
        let appData = {
            balance: 0.00,
            adsCount: 0,
            totalEarned: 0.00,
            totalWithdrawn: 0.00,
            history: []
        };

        let isTimerRunning = false;
        let timerId = null;

        window.onload = function() {
            loadStorage();
            renderApp();
        };

        function loadStorage() {
            const saved = localStorage.getItem('earnpay_mobile_data');
            if (saved) {
                try { appData = JSON.parse(saved); } catch(e){}
            }
        }

        function saveStorage() {
            localStorage.setItem('earnpay_mobile_data', JSON.stringify(appData));
        }

        function renderApp() {
            document.getElementById('appHeaderBalance').innerText = `৳${appData.balance.toFixed(2)}`;
            document.getElementById('dashBalance').innerText = `৳${appData.balance.toFixed(2)}`;
            document.getElementById('dashAdsCount').innerText = `${appData.adsCount} টি`;
            document.getElementById('dashTotalEarned').innerText = `৳${appData.totalEarned.toFixed(2)}`;
            document.getElementById('dashTotalWithdrawn').innerText = `৳${appData.totalWithdrawn.toFixed(2)}`;
            renderHistoryList();
        }

        // Navigation Switcher
        function switchTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.getElementById(`tab-${tabName}`).classList.add('active');

            ['home', 'withdraw', 'history'].forEach(nav => {
                const btn = document.getElementById(`nav-${nav}`);
                if (nav === tabName) {
                    btn.className = "flex flex-col items-center gap-1 text-emerald-400 transition-colors";
                } else {
                    btn.className = "flex flex-col items-center gap-1 text-slate-400 transition-colors";
                }
            });
        }

        // Handle Ad Triggering
        function handleWatchAd() {
            if (isTimerRunning) return;

            if (typeof window.show_12198706 !== 'function') {
                showToast('অ্যাড লোড করা সম্ভব হয়নি! AdBlocker বন্ধ করুন বা রিফ্রেশ দিন।', 'error');
                return;
            }

            const watchBtn = document.getElementById('watchAdBtn');
            watchBtn.disabled = true;

            try {
                window.show_12198706().then(() => {
                    showToast('অ্যাড দেখানো হচ্ছে! কাউন্টডাউন শেষ হওয়া পর্যন্ত অপেক্ষা করুন।', 'info');
                    run10SecTimer();
                }).catch((err) => {
                    console.warn('Monetag execution state:', err);
                    showToast('অ্যাড দেখা সম্পন্ন করুন...', 'info');
                    run10SecTimer();
                });
            } catch(e) {
                showToast('অ্যাড সার্ভারে সমস্যা! আবার চেষ্টা করুন।', 'error');
                watchBtn.disabled = false;
            }
        }

        // 10 Seconds Timer Logic
        function run10SecTimer() {
            isTimerRunning = true;
            const watchBtn = document.getElementById('watchAdBtn');
            const timerContainer = document.getElementById('timerContainer');
            const timerText = document.getElementById('timerText');
            const progressBar = document.getElementById('progressBar');

            watchBtn.classList.add('opacity-50', 'pointer-events-none');
            timerContainer.classList.remove('hidden');

            let secondsLeft = 10;
            timerText.innerText = `${secondsLeft}s`;
            progressBar.style.width = '100%';

            if (timerId) clearInterval(timerId);

            timerId = setInterval(() => {
                secondsLeft--;
                timerText.innerText = `${secondsLeft}s`;
                progressBar.style.width = `${(secondsLeft / 10) * 100}%`;

                if (secondsLeft <= 0) {
                    clearInterval(timerId);

                    appData.balance += 2.00;
                    appData.adsCount += 1;
                    appData.totalEarned += 2.00;
                    saveStorage();
                    renderApp();

                    showToast('অভিনন্দন! আপনার ব্যালেন্সে ৳২.০০ যোগ হয়েছে!', 'success');

                    watchBtn.disabled = false;
                    watchBtn.classList.remove('opacity-50', 'pointer-events-none');
                    timerContainer.classList.add('hidden');
                    isTimerRunning = false;
                }
            }, 1000);
        }

        // Withdrawal Request Function
        function handleWithdraw(e) {
            e.preventDefault();

            const method = document.querySelector('input[name="paymentMethod"]:checked')?.value;
            const account = document.getElementById('accNumber').value.trim();
            const amount = parseFloat(document.getElementById('withdrawAmount').value);

            if (!method || !account || isNaN(amount)) {
                showToast('সঠিক তথ্য দিন!', 'error');
                return;
            }

            if (account.length !== 11) {
                showToast('১১ ডিজিটের সঠিক মোবাইল নম্বর দিন!', 'error');
                return;
            }

            if (amount < 150) {
                showToast('সর্বনিম্ন উইথড্র সীমা ৳১৫০ টাকা!', 'error');
                return;
            }

            if (amount > appData.balance) {
                showToast('আপনার একাউন্টে পর্যাপ্ত ব্যালেন্স নেই!', 'error');
                return;
            }

            appData.balance -= amount;
            appData.totalWithdrawn += amount;

            const dateStr = new Date().toLocaleDateString('bn-BD', { day: '2-digit', month: 'short' });

            appData.history.unshift({
                id: Date.now(),
                date: dateStr,
                method: method,
                account: account,
                amount: amount,
                status: 'পেন্ডিং'
            });

            saveStorage();
            renderApp();
            document.getElementById('mobileWithdrawForm').reset();

            showToast(`৳${amount} টাকা উইথড্র রিকোয়েস্ট সফল হয়েছে!`, 'success');
            switchTab('history');
        }

        // Render History UI
        function renderHistoryList() {
            const container = document.getElementById('historyList');
            container.innerHTML = '';

            if (appData.history.length === 0) {
                container.innerHTML = `<div class="text-center py-8 text-xs text-slate-500 italic">কোনো উইথড্র রেকর্ড নেই</div>`;
                return;
            }

            appData.history.forEach(item => {
                const card = document.createElement('div');
                card.className = "bg-slate-800/80 border border-slate-700/60 p-3 rounded-xl flex items-center justify-between text-xs";
                card.innerHTML = `
                    <div class="space-y-0.5">
                        <div class="font-bold text-white flex items-center gap-1.5">
                            <span>${item.method}</span>
                            <span class="text-[10px] text-slate-400 font-normal">(${item.account})</span>
                        </div>
                        <div class="text-[10px] text-slate-400">${item.date}</div>
                    </div>
                    <div class="text-right space-y-0.5">
                        <div class="font-bold text-emerald-400">৳${item.amount.toFixed(2)}</div>
                        <span class="inline-block px-2 py-0.5 rounded-full text-[9px] bg-amber-500/10 text-amber-400 border border-amber-500/20">${item.status}</span>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function clearHistory() {
            if (confirm('হিস্ট্রি ক্লিয়ার করতে চান?')) {
                appData.history = [];
                saveStorage();
                renderApp();
                showToast('হিস্ট্রি মুছে ফেলা হয়েছে', 'info');
            }
        }

        // Notification Toast Engine
        function showToast(msg, type = 'info') {
            const toast = document.getElementById('appToast');
            const toastMsg = document.getElementById('toastMsg');
            const toastIcon = document.getElementById('toastIcon');

            toastMsg.innerText = msg;
            
            if (type === 'success') {
                toastIcon.className = "fa-solid fa-circle-check text-emerald-400 text-base";
            } else if (type === 'error') {
                toastIcon.className = "fa-solid fa-circle-exclamation text-rose-400 text-base";
            } else {
                toastIcon.className = "fa-solid fa-circle-info text-blue-400 text-base";
            }

            toast.classList.remove('opacity-0', 'translate-y-4');
            toast.classList.add('opacity-100', 'translate-y-0');

            setTimeout(() => {
                toast.classList.remove('opacity-100', 'translate-y-0');
                toast.classList.add('opacity-0', 'translate-y-4');
            }, 3000);
        }
    </script>
</body>
</html>
