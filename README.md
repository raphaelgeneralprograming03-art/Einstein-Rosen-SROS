
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Directed Singularity Mechanism - Wormhole Simulation</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        cyber: {
                            cyan: '#00f0ff',
                            crimson: '#ff1744',
                            amber: '#ffab00',
                            dark: '#030712',
                            panel: '#0b0f19'
                        }
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts: Orbitron & Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Orbitron:wght@400;600;800;900&display=swap" rel="stylesheet">
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #030712;
            color: #f3f4f6;
            overflow-x: hidden;
        }
        .font-orbitron {
            font-family: 'Orbitron', sans-serif;
        }
        .glass-panel {
            background: rgba(11, 15, 25, 0.88);
            backdrop-filter: blur(14px);
            border: 1px solid rgba(0, 240, 255, 0.15);
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.6), inset 0 0 20px rgba(0, 240, 255, 0.03);
        }
        .glass-panel-crimson {
            border: 1px solid rgba(255, 23, 68, 0.25);
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.6), inset 0 0 20px rgba(255, 23, 68, 0.04);
        }
        .glow-text-cyan {
            text-shadow: 0 0 12px rgba(0, 240, 255, 0.7);
        }
        .glow-text-crimson {
            text-shadow: 0 0 12px rgba(255, 23, 68, 0.7);
        }
        .glow-text-amber {
            text-shadow: 0 0 12px rgba(255, 171, 0, 0.7);
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #030712;
        }
        ::-webkit-scrollbar-thumb {
            background: #1f2937;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #374151;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <header class="glass-panel border-b border-cyan-500/20 px-6 py-4 flex flex-wrap justify-between items-center z-20">
        <div class="flex items-center space-x-3">
            <div class="w-10 h-10 rounded-lg bg-cyan-500/10 border border-cyan-400/40 flex items-center justify-center text-cyan-400">
                <i class="fa-solid fa-wormhole text-xl animate-pulse"></i>
            </div>
            <div>
                <h1 class="font-orbitron font-extrabold text-xl tracking-wider text-white glow-text-cyan">EINSTEIN-ROSEN SROS</h1>
                <p class="text-xs text-cyan-400/70 tracking-widest uppercase">Directed Singularity Mechanism (Ship A & Ship B)</p>
            </div>
        </div>
        <div class="flex items-center space-x-4 mt-3 md:mt-0">
            <button id="codex-btn" class="px-4 py-2 rounded-lg bg-slate-800/80 hover:bg-slate-700 border border-cyan-500/30 text-cyan-300 text-sm font-orbitron transition flex items-center space-x-2">
                <i class="fa-solid fa-book-atlas"></i>
                <span>Bridge Protocol Codex</span>
            </button>
            <div class="flex items-center space-x-2 bg-slate-900/90 border border-slate-700 px-3 py-1.5 rounded-lg">
                <span id="bridge-status-indicator" class="w-2.5 h-2.5 rounded-full bg-amber-400 animate-ping"></span>
                <span id="bridge-status-text" class="text-xs font-orbitron text-amber-400 font-bold">STANDBY</span>
            </div>
        </div>
    </header>

    <div class="flex-1 grid grid-cols-1 lg:grid-cols-12 gap-4 p-4 max-w-[1920px] w-full mx-auto">

        <!-- Simulation Canvas Panel (Cols 1-8) -->
        <div class="lg:col-span-8 glass-panel rounded-2xl p-4 flex flex-col relative overflow-hidden min-h-[550px] lg:min-h-[750px]">
            <div class="flex justify-between items-center mb-3 z-10">
                <div class="flex items-center space-x-2">
                    <i class="fa-solid fa-satellite text-cyan-400"></i>
                    <h2 class="font-orbitron font-semibold text-sm tracking-wider text-cyan-300">METRIC SPACE-TIME WARPING & SROS OPTICAL ANCHORING</h2>
                </div>
                <div class="flex items-center space-x-2">
                    <button id="pause-btn" class="px-3 py-1 bg-cyan-950/60 hover:bg-cyan-900/80 border border-cyan-500/40 text-cyan-300 rounded text-xs font-orbitron transition">
                        <i class="fa-solid fa-pause mr-1"></i> Pause
                    </button>
                    <button id="reset-btn" class="px-3 py-1 bg-slate-800 hover:bg-slate-700 border border-slate-600 text-slate-300 rounded text-xs font-orbitron transition">
                        <i class="fa-solid fa-rotate-right mr-1"></i> Reset
                    </button>
                </div>
            </div>

            <!-- Canvas Container -->
            <div class="flex-1 relative rounded-xl overflow-hidden border border-cyan-500/10 bg-black/50 flex items-center justify-center">
                <canvas id="simCanvas" class="w-full h-full block cursor-crosshair"></canvas>
                
                <!-- Overlay Legends -->
                <div class="absolute bottom-3 left-3 pointer-events-none bg-slate-950/85 backdrop-blur-md p-3 rounded-xl border border-slate-800 text-xs space-y-1.5 text-slate-300">
                    <div class="flex items-center space-x-2">
                        <span class="w-3 h-3 rounded-full bg-rose-600 inline-block shadow-[0_0_8px_#ff1744]"></span>
                        <span class="font-orbitron text-rose-400">Ship A (Attractor / Gravity Collapse)</span>
                    </div>
                    <div class="flex items-center space-x-2">
                        <span class="w-3 h-3 rounded-full bg-cyan-400 inline-block shadow-[0_0_8px_#00f0ff]"></span>
                        <span class="font-orbitron text-cyan-300">Ship B (Expansor / Casimir Negative Pressure)</span>
                    </div>
                    <div class="flex items-center space-x-2">
                        <span class="w-3 h-3 rounded-full bg-amber-400 inline-block shadow-[0_0_8px_#ffab00]"></span>
                        <span class="font-orbitron text-amber-300">SROS Full-Duplex Optical Laser Link</span>
                    </div>
                </div>
            </div>
        </div>

        <div class="lg:col-span-4 flex flex-col space-y-4">
            
            <!-- Real-Time Telemetry Card -->
            <div class="glass-panel rounded-2xl p-4 space-y-4">
                <h3 class="font-orbitron font-semibold text-xs tracking-wider text-cyan-400 flex items-center space-x-2">
                    <i class="fa-solid fa-chart-line"></i>
                    <span>WORMHOLE METRICS & TELEMETRY</span>
                </h3>

                <div class="grid grid-cols-2 gap-3">
                    <div class="bg-slate-950/60 p-3 rounded-xl border border-cyan-500/10">
                        <div class="text-[10px] text-slate-400 font-orbitron">WORMHOLE STABILITY</div>
                        <div id="val-stability" class="text-lg font-orbitron font-bold text-amber-400 glow-text-amber">0.0%</div>
                    </div>
                    <div class="bg-slate-950/60 p-3 rounded-xl border border-cyan-500/10">
                        <div class="text-[10px] text-slate-400 font-orbitron">THROAT DIAMETER</div>
                        <div id="val-throat" class="text-lg font-orbitron font-bold text-cyan-300 glow-text-cyan">0.00 km</div>
                    </div>
                    <div class="bg-slate-950/60 p-3 rounded-xl border border-cyan-500/10">
                        <div class="text-[10px] text-slate-400 font-orbitron">SROS ALIGNMENT</div>
                        <div id="val-sros" class="text-base font-orbitron font-bold text-emerald-400">99.99%</div>
                    </div>
                    <div class="bg-slate-950/60 p-3 rounded-xl border border-cyan-500/10">
                        <div class="text-[10px] text-slate-400 font-orbitron">QUANTUM INDEX</div>
                        <div id="val-quantum" class="text-base font-orbitron font-bold text-rose-400 glow-text-crimson">1.00 $\Phi$</div>
                    </div>
                </div>

                <!-- Bridge Integrity Progress Bar -->
                <div class="bg-slate-950/80 p-3.5 rounded-xl border border-slate-800 space-y-2">
                    <div class="flex justify-between items-center text-xs font-orbitron">
                        <span class="text-slate-300">THROAT HORIZON INTEGRITY</span>
                        <span id="integrity-label" class="text-amber-400 font-bold">DISCONNECTED</span>
                    </div>
                    <div class="w-full bg-slate-800 h-2.5 rounded-full overflow-hidden">
                        <div id="bar-integrity" class="bg-gradient-to-r from-rose-500 via-amber-400 to-cyan-400 h-full transition-all duration-300" style="width: 0%;"></div>
                    </div>
                </div>
            </div>

            <!-- Action Command Deck Card -->
            <div class="glass-panel rounded-2xl p-4 space-y-3">
                <h3 class="font-orbitron font-semibold text-xs tracking-wider text-cyan-400 flex items-center space-x-2">
                    <i class="fa-solid fa-terminal"></i>
                    <span>SEQUENCE COMMAND DECK</span>
                </h3>

                <div class="grid grid-cols-2 gap-2">
                    <button id="btn-puncture" class="px-3 py-2.5 rounded-xl bg-rose-950/60 hover:bg-rose-900/80 border border-rose-500/40 text-rose-300 text-xs font-orbitron font-bold transition flex items-center justify-center space-x-2 shadow-lg shadow-rose-950/50">
                        <i class="fa-solid fa-bullseye"></i>
                        <span>Puncture Spacetime</span>
                    </button>
                    <button id="btn-expand" class="px-3 py-2.5 rounded-xl bg-cyan-950/60 hover:bg-cyan-900/80 border border-cyan-500/40 text-cyan-300 text-xs font-orbitron font-bold transition flex items-center justify-center space-x-2 shadow-lg shadow-cyan-950/50">
                        <i class="fa-solid fa-expand"></i>
                        <span>Expand Throat</span>
                    </button>
                    <button id="btn-stabilize" class="px-3 py-2.5 rounded-xl bg-amber-950/60 hover:bg-amber-900/80 border border-amber-500/40 text-amber-300 text-xs font-orbitron font-bold transition flex items-center justify-center space-x-2 shadow-lg shadow-amber-950/50">
                        <i class="fa-solid fa-shield-halved"></i>
                        <span>Stabilize Bridge</span>
                    </button>
                    <button id="btn-collapse" class="px-3 py-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 border border-slate-600 text-slate-300 text-xs font-orbitron font-bold transition flex items-center justify-center space-x-2">
                        <i class="fa-solid fa-triangle-exclamation text-rose-400"></i>
                        <span>Emergency Abort</span>
                    </button>
                </div>
            </div>

            <div class="glass-panel rounded-2xl p-4 space-y-3.5">
                <h3 class="font-orbitron font-semibold text-xs tracking-wider text-cyan-400 flex items-center space-x-2">
                    <i class="fa-solid fa-sliders"></i>
                    <span>SUBSYSTEM PARAMETERS</span>
                </h3>

                <!-- Ship A Positive Mass Energy -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-orbitron">
                        <span class="text-rose-400">Ship A Positive Mass-Energy</span>
                        <span id="val-mass-a" class="text-rose-400 font-bold">50%</span>
                    </div>
                    <input id="range-mass-a" type="range" min="0" max="100" step="1" value="50" 
                           class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-rose-500">
                </div>

                <!-- Ship B Negative Pressure (Casimir) -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-orbitron">
                        <span class="text-cyan-400">Ship B Negative Pressure (Casimir)</span>
                        <span id="val-casimir-b" class="text-cyan-400 font-bold">50%</span>
                    </div>
                    <input id="range-casimir-b" type="range" min="0" max="100" step="1" value="50" 
                           class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-cyan-400">
                </div>

                <!-- SROS Optical Laser Alignment -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-orbitron">
                        <span class="text-amber-400">SROS Laser Precision Alignment</span>
                        <span id="val-sros-slider" class="text-amber-400 font-bold">90%</span>
                    </div>
                    <input id="range-sros" type="range" min="0" max="100" step="0.5" value="90" 
                           class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-amber-400">
                </div>

                <!-- Inter-ship Distance -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-orbitron">
                        <span class="text-slate-300">Inter-Ship Spatial Span</span>
                        <span id="val-distance" class="text-slate-300 font-bold">4.2 Light-Seconds</span>
                    </div>
                    <input id="range-distance" type="range" min="1.0" max="10.0" step="0.1" value="4.2" 
                           class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-slate-400">
                </div>

                <!-- Quick Presets -->
                <div class="pt-2 border-t border-slate-800/80">
                    <div class="text-[10px] text-slate-400 font-orbitron mb-2">OPERATIONAL PRESETS</div>
                    <div class="grid grid-cols-3 gap-2">
                        <button id="preset-stable" class="px-2 py-1.5 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-cyan-300 text-[10px] font-orbitron transition">Stable Bridge</button>
                        <button id="preset-collapse" class="px-2 py-1.5 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-rose-400 text-[10px] font-orbitron transition">Grav Collapse</button>
                        <button id="preset-decohere" class="px-2 py-1.5 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-amber-300 text-[10px] font-orbitron transition">Decoherence</button>
                    </div>
                </div>
            </div>

        </div>

    </div>

    <div id="codex-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/85 backdrop-blur-md hidden p-4">
        <div class="glass-panel rounded-2xl max-w-2xl w-full p-6 space-y-4 max-h-[85vh] overflow-y-auto border border-cyan-500/40 relative">
            <div class="flex justify-between items-center border-b border-cyan-500/20 pb-3">
                <div class="flex items-center space-x-2 text-cyan-400 font-orbitron font-bold text-lg">
                    <i class="fa-solid fa-book-atlas"></i>
                    <span>DIRECTED SINGULARITY MECHANISM CODEX</span>
                </div>
                <button id="close-codex" class="text-slate-400 hover:text-white text-xl p-1">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
            
            <div class="space-y-4 text-sm text-slate-300 leading-relaxed">
                <div>
                    <h4 class="font-orbitron font-bold text-rose-400 mb-1">1. Ship A (The Attractor / Positive Mass-Energy)</h4>
                    <p>Utilizes an onboard high-output Magnetohydrodynamic (MHD) reactor to concentrate extreme positive mass-energy into a focal point. This creates a steep gravitational well that collapses and punctures the spacetime metric tensor, forming the wormhole's entry mouth.</p>
                </div>

                <div>
                    <h4 class="font-orbitron font-bold text-cyan-300 mb-1">2. Ship B (The Expansor / Casimir Negative Pressure)</h4>
                    <p>Operates in tandem by projecting macroscopic negative energy densities via Casimir vacuum suppression plates. This negative pressure repels the local metric, expanding the Point B exit and preventing immediate gravitational pinch-off into a standard black hole.</p>
                </div>

                <div>
                    <h4 class="font-orbitron font-bold text-amber-300 mb-1">3. SROS (Synchronized Optical Network System)</h4>
                    <p>Maintains millimeter-precision alignment across vast spatial spans using full-duplex high-frequency optical laser links. SROS ensures quantum entanglement and fluctuation resonance between Ship A and Ship B, sustaining the Einstein-Rosen bridge throat.</p>
                </div>
            </div>

            <div class="pt-4 border-t border-slate-800 flex justify-end">
                <button id="close-codex-btn" class="px-5 py-2 bg-cyan-500 hover:bg-cyan-400 text-slate-950 font-orbitron font-bold rounded-lg text-xs transition">
                    Acknowledge Protocol
                </button>
            </div>
        </div>
    </div>

    <script>
        window.addEventListener('DOMContentLoaded', () => {
            const canvas = document.getElementById('simCanvas');
            const ctx = canvas.getContext('2d');

            // UI Elements
            const massASlider = document.getElementById('range-mass-a');
            const massALabel = document.getElementById('val-mass-a');
            const casimirBSlider = document.getElementById('range-casimir-b');
            const casimirBLabel = document.getElementById('val-casimir-b');
            const srosSlider = document.getElementById('range-sros');
            const srosLabel = document.getElementById('val-sros-slider');
            const distanceSlider = document.getElementById('range-distance');
            const distanceLabel = document.getElementById('val-distance');

            const valStability = document.getElementById('val-stability');
            const valThroat = document.getElementById('val-throat');
            const valSros = document.getElementById('val-sros');
            const valQuantum = document.getElementById('val-quantum');
            const integrityLabel = document.getElementById('integrity-label');
            const barIntegrity = document.getElementById('bar-integrity');
            const bridgeStatusIndicator = document.getElementById('bridge-status-indicator');
            const bridgeStatusText = document.getElementById('bridge-status-text');

            const btnPuncture = document.getElementById('btn-puncture');
            const btnExpand = document.getElementById('btn-expand');
            const btnStabilize = document.getElementById('btn-stabilize');
            const btnCollapse = document.getElementById('btn-collapse');
            const pauseBtn = document.getElementById('pause-btn');
            const resetBtn = document.getElementById('reset-btn');

            const presetStable = document.getElementById('preset-stable');
            const presetCollapse = document.getElementById('preset-collapse');
            const presetDecohere = document.getElementById('preset-decohere');

            const codexBtn = document.getElementById('codex-btn');
            const codexModal = document.getElementById('codex-modal');
            const closeCodex = document.getElementById('close-codex');
            const closeCodexBtn = document.getElementById('close-codex-btn');

            // Simulation State
            let isPaused = false;
            let time = 0;
            let throatPulse = 0;

            // Parameters
            let massA = parseFloat(massASlider.value);
            let casimirB = parseFloat(casimirBSlider.value);
            let srosAlignment = parseFloat(srosSlider.value);
            let intershipDistance = parseFloat(distanceSlider.value);

            // Particle arrays for wormhole throat & laser flux
            const throatParticles = [];
            const laserPhotons = [];
            const numThroatParticles = 80;

            for (let i = 0; i < numThroatParticles; i++) {
                throatParticles.push({
                    progress: Math.random(),
                    speed: 0.005 + Math.random() * 0.01,
                    radiusOffset: (Math.random() - 0.5) * 15,
                    angle: Math.random() * Math.PI * 2
                });
            }

            // Resize Canvas
            function resizeCanvas() {
                const rect = canvas.parentElement.getBoundingClientRect();
                canvas.width = rect.width * window.devicePixelRatio;
                canvas.height = rect.height * window.devicePixelRatio;
                ctx.scale(window.devicePixelRatio, window.devicePixelRatio);
            }
            window.addEventListener('resize', resizeCanvas);
            resizeCanvas();

            // Event Listeners for Sliders
            massASlider.addEventListener('input', (e) => {
                massA = parseFloat(e.target.value);
                massALabel.textContent = massA + '%';
            });
            casimirBSlider.addEventListener('input', (e) => {
                casimirB = parseFloat(e.target.value);
                casimirBLabel.textContent = casimirB + '%';
            });
            srosSlider.addEventListener('input', (e) => {
                srosAlignment = parseFloat(e.target.value);
                srosLabel.textContent = srosAlignment.toFixed(1) + '%';
            });
            distanceSlider.addEventListener('input', (e) => {
                intershipDistance = parseFloat(e.target.value);
                distanceLabel.textContent = intershipDistance.toFixed(1) + ' Light-Seconds';
            });

            // Action Buttons
            btnPuncture.addEventListener('click', () => {
                massA = 85; massASlider.value = 85; massALabel.textContent = '85%';
                casimirB = 40; casimirBSlider.value = 40; casimirBLabel.textContent = '40%';
                srosAlignment = 95; srosSlider.value = 95; srosLabel.textContent = '95.0%';
            });

            btnExpand.addEventListener('click', () => {
                massA = 90; massASlider.value = 90; massALabel.textContent = '90%';
                casimirB = 92; casimirBSlider.value = 92; casimirBLabel.textContent = '92%';
                srosAlignment = 98; srosSlider.value = 98; srosLabel.textContent = '98.0%';
            });

            btnStabilize.addEventListener('click', () => {
                massA = 80; massASlider.value = 80; massALabel.textContent = '80%';
                casimirB = 80; casimirBSlider.value = 80; casimirBLabel.textContent = '80%';
                srosAlignment = 99.9; srosSlider.value = 99.9; srosLabel.textContent = '99.9%';
            });

            btnCollapse.addEventListener('click', () => {
                massA = 0; massASlider.value = 0; massALabel.textContent = '0%';
                casimirB = 0; casimirBSlider.value = 0; casimirBLabel.textContent = '0%';
                srosAlignment = 10; srosSlider.value = 10; srosLabel.textContent = '10.0%';
            });

            pauseBtn.addEventListener('click', () => {
                isPaused = !isPaused;
                pauseBtn.innerHTML = isPaused ? '<i class="fa-solid fa-play mr-1"></i> Resume' : '<i class="fa-solid fa-pause mr-1"></i> Pause';
            });

            resetBtn.addEventListener('click', () => {
                massA = 50; massASlider.value = 50; massALabel.textContent = '50%';
                casimirB = 50; casimirBSlider.value = 50; casimirBLabel.textContent = '50%';
                srosAlignment = 90; srosSlider.value = 90; srosLabel.textContent = '90.0%';
                intershipDistance = 4.2; distanceSlider.value = 4.2; distanceLabel.textContent = '4.2 Light-Seconds';
                time = 0;
            });

            // Presets
            presetStable.addEventListener('click', () => {
                massA = 85; massASlider.value = 85; massALabel.textContent = '85%';
                casimirB = 85; casimirBSlider.value = 85; casimirBLabel.textContent = '85%';
                srosAlignment = 99.5; srosSlider.value = 99.5; srosLabel.textContent = '99.5%';
                intershipDistance = 3.5; distanceSlider.value = 3.5; distanceLabel.textContent = '3.5 Light-Seconds';
            });

            presetCollapse.addEventListener('click', () => {
                massA = 98; massASlider.value = 98; massALabel.textContent = '98%';
                casimirB = 10; casimirBSlider.value = 10; casimirBLabel.textContent = '10%';
                srosAlignment = 45; srosSlider.value = 45; srosLabel.textContent = '45.0%';
                intershipDistance = 5.0; distanceSlider.value = 5.0; distanceLabel.textContent = '5.0 Light-Seconds';
            });

            presetDecohere.addEventListener('click', () => {
                massA = 40; massASlider.value = 40; massALabel.textContent = '40%';
                casimirB = 70; casimirBSlider.value = 70; casimirBLabel.textContent = '70%';
                srosAlignment = 25; srosSlider.value = 25; srosLabel.textContent = '25.0%';
                intershipDistance = 8.0; distanceSlider.value = 8.0; distanceLabel.textContent = '8.0 Light-Seconds';
            });

            // Codex Modal Controls
            codexBtn.addEventListener('click', () => codexModal.classList.remove('hidden'));
            closeCodex.addEventListener('click', () => codexModal.classList.add('hidden'));
            closeCodexBtn.addEventListener('click', () => codexModal.classList.add('hidden'));

            // Physics calculation & Telemetry update
            function updatePhysicsAndTelemetry() {
                // Balance between positive mass energy and negative Casimir pressure
                const balance = 1.0 - Math.abs(massA - casimirB) / 100.0;
                const alignmentFactor = srosAlignment / 100.0;
                const distancePenalty = 1.0 / (intershipDistance * 0.3);

                // Wormhole stability %
                let stability = Math.max(0, Math.min(100, (massA * 0.4 + casimirB * 0.4) * balance * alignmentFactor * distancePenalty));
                if (massA < 20 || casimirB < 20) stability = 0;

                // Throat diameter proportional to stability and energy product
                const throatDiameter = (stability > 5) ? (stability * 0.24 * (Math.min(massA, casimirB) / 50)) : 0.0;

                // Update HUD readouts
                valStability.textContent = stability.toFixed(1) + '%';
                valThroat.textContent = throatDiameter.toFixed(2) + ' km';
                valSros.textContent = srosAlignment.toFixed(1) + '%';
                
                const quantumIndex = 1.0 + (massA * 0.015) - (casimirB * 0.01) + Math.sin(time * 0.05) * 0.05;
                valQuantum.textContent = quantumIndex.toFixed(2) + ' \u03A6';

                barIntegrity.style.width = stability + '%';

                if (stability > 75) {
                    bridgeStatusIndicator.className = 'w-2.5 h-2.5 rounded-full bg-cyan-400 animate-pulse';
                    bridgeStatusText.className = 'text-xs font-orbitron text-cyan-300 font-bold';
                    bridgeStatusText.textContent = 'BRIDGE STABLE';
                    integrityLabel.className = 'text-cyan-300 font-bold';
                    integrityLabel.textContent = 'CONNECTED (100%)';
                } else if (stability > 20) {
                    bridgeStatusIndicator.className = 'w-2.5 h-2.5 rounded-full bg-amber-400 animate-pulse';
                    bridgeStatusText.className = 'text-xs font-orbitron text-amber-400 font-bold';
                    bridgeStatusText.textContent = 'RESONATING';
                    integrityLabel.className = 'text-amber-400 font-bold';
                    integrityLabel.textContent = 'FLUCTUATING';
                } else {
                    bridgeStatusIndicator.className = 'w-2.5 h-2.5 rounded-full bg-rose-500';
                    bridgeStatusText.className = 'text-xs font-orbitron text-rose-500 font-bold';
                    bridgeStatusText.textContent = 'COLLAPSED';
                    integrityLabel.className = 'text-rose-500 font-bold';
                    integrityLabel.textContent = 'DISCONNECTED';
                }

                return { stability, throatDiameter };
            }

            // Main Animation Render Loop
            function animate() {
                const width = canvas.parentElement.clientWidth;
                const height = canvas.parentElement.clientHeight;

                ctx.clearRect(0, 0, width, height);

                const physics = updatePhysicsAndTelemetry();
                const stability = physics.stability;
                const throatDiameter = physics.throatDiameter;

                if (!isPaused) {
                    time += 1;
                    throatPulse += 0.04;
                }

                // Ship positions
                const shipAX = width * 0.28;
                const shipAY = height * 0.5;
                const shipBX = width * 0.72;
                const shipBY = height * 0.5;

                // 1. Render Warped Spacetime Grid
                ctx.save();
                const gridSpacing = 45;
                ctx.lineWidth = 1;

                for (let x = gridSpacing; x < width; x += gridSpacing) {
                    ctx.beginPath();
                    for (let y = 0; y <= height; y += 15) {
                        let px = x;
                        let py = y;

                        // Distortion towards Ship A (Gravitational Well / Collapse)
                        const dxA = px - shipAX;
                        const dyA = py - shipAY;
                        const distA = Math.sqrt(dxA * dxA + dyA * dyA);
                        const wellA = (massA * 1.8) / (distA * 0.08 + 15);
                        px += (dxA / (distA + 1)) * wellA * 12;
                        py += (dyA / (distA + 1)) * wellA * 12;

                        // Distortion near Ship B (Casimir Negative Pressure Expansion Dome)
                        const dxB = px - shipBX;
                        const dyB = py - shipBY;
                        const distB = Math.sqrt(dxB * dxB + dyB * dyB);
                        const domeB = (casimirB * 1.5) / (distB * 0.08 + 15);
                        px -= (dxB / (distB + 1)) * domeB * 10;
                        py -= (dyB / (distB + 1)) * domeB * 10;

                        if (y === 0) ctx.moveTo(px, py);
                        else ctx.lineTo(px, py);
                    }
                    ctx.strokeStyle = 'rgba(0, 240, 255, 0.08)';
                    ctx.stroke();
                }

                for (let y = gridSpacing; y < height; y += gridSpacing) {
                    ctx.beginPath();
                    for (let x = 0; x <= width; x += 15) {
                        let px = x;
                        let py = y;

                        const dxA = px - shipAX;
                        const dyA = py - shipAY;
                        const distA = Math.sqrt(dxA * dxA + dyA * dyA);
                        const wellA = (massA * 1.8) / (distA * 0.08 + 15);
                        px += (dxA / (distA + 1)) * wellA * 12;
                        py += (dyA / (distA + 1)) * wellA * 12;

                        const dxB = px - shipBX;
                        const dyB = py - shipBY;
                        const distB = Math.sqrt(dxB * dxB + dyB * dyB);
                        const domeB = (casimirB * 1.5) / (distB * 0.08 + 15);
                        px -= (dxB / (distB + 1)) * domeB * 10;
                        py -= (dyB / (distB + 1)) * domeB * 10;

                        if (x === 0) ctx.moveTo(px, py);
                        else ctx.lineTo(px, py);
                    }
                    ctx.strokeStyle = 'rgba(0, 240, 255, 0.08)';
                    ctx.stroke();
                }
                ctx.restore();

                // 2. Render Wormhole Einstein-Rosen Bridge Throat (if stability > 0)
                if (stability > 5) {
                    ctx.save();
                    const bridgeRadius = Math.max(15, throatDiameter * 18);
                    
                    // Tunnel Gradient Glow
                    const bridgeGradient = ctx.createRadialGradient(shipAX, shipAY, 5, shipAX, shipAY, bridgeRadius * 2);
                    bridgeGradient.addColorStop(0, 'rgba(255, 23, 68, 0.7)');
                    bridgeGradient.addColorStop(0.5, 'rgba(255, 171, 0, 0.4)');
                    bridgeGradient.addColorStop(1, 'transparent');

                    ctx.fillStyle = bridgeGradient;
                    ctx.beginPath();
                    ctx.arc(shipAX, shipAY, bridgeRadius * 2, 0, Math.PI * 2);
                    ctx.fill();

                    const bridgeExitGradient = ctx.createRadialGradient(shipBX, shipBY, 5, shipBX, shipBY, bridgeRadius * 2);
                    bridgeExitGradient.addColorStop(0, 'rgba(0, 240, 255, 0.8)');
                    bridgeExitGradient.addColorStop(0.5, 'rgba(0, 150, 255, 0.4)');
                    bridgeExitGradient.addColorStop(1, 'transparent');

                    ctx.fillStyle = bridgeExitGradient;
                    ctx.beginPath();
                    ctx.arc(shipBX, shipBY, bridgeRadius * 2, 0, Math.PI * 2);
                    ctx.fill();

                    // Connecting Throat Tunnel Curve
                    ctx.beginPath();
                    ctx.moveTo(shipAX, shipAY - bridgeRadius);
                    ctx.bezierCurveTo(width * 0.4, shipAY - bridgeRadius * 2.5, width * 0.6, shipBY - bridgeRadius * 2.5, shipBX, shipBY - bridgeRadius);
                    ctx.lineTo(shipBX, shipBY + bridgeRadius);
                    ctx.bezierCurveTo(width * 0.6, shipBY + bridgeRadius * 2.5, width * 0.4, shipAY + bridgeRadius * 2.5, shipAX, shipAY + bridgeRadius);
                    ctx.closePath();
                    ctx.fillStyle = 'rgba(0, 240, 255, 0.12)';
                    ctx.strokeStyle = 'rgba(0, 240, 255, 0.4)';
                    ctx.lineWidth = 2;
                    ctx.fill();
                    ctx.stroke();

                    // Swirling Throat Particles
                    throatParticles.forEach(p => {
                        if (!isPaused) {
                            p.progress += p.speed;
                            if (p.progress > 1) p.progress = 0;
                        }
                        // Cubic bezier interpolation between Ship A and Ship B
                        const t = p.progress;
                        const cx1 = width * 0.4;
                        const cy1 = shipAY - bridgeRadius * 1.5;
                        const cx2 = width * 0.6;
                        const cy2 = shipBY - bridgeRadius * 1.5;

                        const bx = Math.pow(1-t, 3) * shipAX + 3 * Math.pow(1-t, 2) * t * cx1 + 3 * (1-t) * Math.pow(t, 2) * cx2 + Math.pow(t, 3) * shipBX;
                        const by = Math.pow(1-t, 3) * shipAY + 3 * Math.pow(1-t, 2) * t * cy1 + 3 * (1-t) * Math.pow(t, 2) * cy2 + Math.pow(t, 3) * shipBY;

                        const currRadius = bridgeRadius * Math.sin(t * Math.PI);
                        const px = bx + Math.cos(p.angle + time * 0.1) * currRadius;
                        const py = by + Math.sin(p.angle + time * 0.1) * (currRadius * 0.5);

                        ctx.beginPath();
                        ctx.arc(px, py, 2, 0, Math.PI * 2);
                        ctx.fillStyle = t < 0.5 ? '#ff1744' : '#00f0ff';
                        ctx.shadowColor = t < 0.5 ? '#ff1744' : '#00f0ff';
                        ctx.shadowBlur = 8;
                        ctx.fill();
                        ctx.shadowBlur = 0;
                    });

                    ctx.restore();
                }

                // 3. Render SROS Full-Duplex Optical Laser Links
                ctx.save();
                ctx.beginPath();
                ctx.moveTo(shipAX, shipAY);
                ctx.lineTo(shipBX, shipBY);
                ctx.strokeStyle = `rgba(255, 171, 0, ${0.3 + (srosAlignment / 200)})`;
                ctx.lineWidth = 2;
                ctx.setLineDash([6, 6]);
                ctx.stroke();
                ctx.setLineDash([]);

                // Pulsing Laser Photons
                const pulseOffset = (time * 6) % 100;
                for (let i = 0; i < 3; i++) {
                    const lDist = (shipBX - shipAX);
                    const lx = shipAX + ((i * 33 + pulseOffset) % lDist);
                    const ly = shipAY;
                    ctx.beginPath();
                    ctx.arc(lx, ly, 3.5, 0, Math.PI * 2);
                    ctx.fillStyle = '#ffab00';
                    ctx.shadowColor = '#ffab00';
                    ctx.shadowBlur = 10;
                    ctx.fill();
                    ctx.shadowBlur = 0;
                }
                ctx.restore();

                // 4. Render Ship A (The Attractor - Crimson Glow)
                ctx.save();
                ctx.translate(shipAX, shipAY);
                
                const auraA = ctx.createRadialGradient(0, 0, 0, 0, 0, 35);
                auraA.addColorStop(0, 'rgba(255, 23, 68, 0.8)');
                auraA.addColorStop(1, 'transparent');
                ctx.fillStyle = auraA;
                ctx.beginPath();
                ctx.arc(0, 0, 35, 0, Math.PI * 2);
                ctx.fill();

                // Ship A Hull (Crimson Dart)
                ctx.beginPath();
                ctx.moveTo(14, 0);
                ctx.lineTo(-10, -10);
                ctx.lineTo(-4, 0);
                ctx.lineTo(-10, 10);
                ctx.closePath();
                ctx.fillStyle = '#ffffff';
                ctx.shadowColor = '#ff1744';
                ctx.shadowBlur = 15;
                ctx.fill();
                ctx.shadowBlur = 0;

                // Label
                ctx.fillStyle = '#ff1744';
                ctx.font = '11px Orbitron';
                ctx.textAlign = 'center';
                ctx.fillText('[ SHIP A: ATTRACTOR ]', 0, -25);
                ctx.restore();

                // 5. Render Ship B (The Expansor - Cyan Glow)
                ctx.save();
                ctx.translate(shipBX, shipBY);

                const auraB = ctx.createRadialGradient(0, 0, 0, 0, 0, 35);
                auraB.addColorStop(0, 'rgba(0, 240, 255, 0.8)');
                auraB.addColorStop(1, 'transparent');
                ctx.fillStyle = auraB;
                ctx.beginPath();
                ctx.arc(0, 0, 35, 0, Math.PI * 2);
                ctx.fill();

                // Ship B Hull (Cyan Dart facing opposite)
                ctx.beginPath();
                ctx.moveTo(-14, 0);
                ctx.lineTo(10, -10);
                ctx.lineTo(4, 0);
                ctx.lineTo(10, 10);
                ctx.closePath();
                ctx.fillStyle = '#ffffff';
                ctx.shadowColor = '#00f0ff';
                ctx.shadowBlur = 15;
                ctx.fill();
                ctx.shadowBlur = 0;

                // Label
                ctx.fillStyle = '#00f0ff';
                ctx.font = '11px Orbitron';
                ctx.textAlign = 'center';
                ctx.fillText('[ SHIP B: EXPANSOR ]', 0, -25);
                ctx.restore();

                requestAnimationFrame(animate);
            }

            requestAnimationFrame(animate);
        });
    </script>
</body>
</html>
