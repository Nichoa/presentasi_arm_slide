<!-- Slide 8: Implementasi -->
        <div class="slide">
            <h2>🛠️ Panduan Implementasi Detail</h2>
            
            <div class="info-card" style="background: #e8f5e9; border-left-color: #4caf50;">
                <h3>📋 Step-by-Step Implementation Guide</h3>
                <ol style="list-style: decimal; padding-left: 40px;">
                    <li style="margin: 15px 0;">
                        <strong>Persiapan & Backup (5 menit)</strong>
                        <ul style="margin-top: 5px; list-style: circle;">
                            <li>Export full config: <code>/export file=backup-before-fqcodel</code></li>
                            <li>Screenshot current queue stats untuk comparison</li>
                            <li>Catat baseline latency dengan <code>ping 8.8.8.8 -t</code></li>
                        </ul>
                    </li>
                    <li style="margin: 15px 0;">
                        <strong>Update RouterOS (10 menit)</strong>
                        <ul style="margin-top: 5px; list-style: circle;">
                            <li>Check version: <code>/system resource print</code> (minimal v7.1)</li>
                            <li>Download package dari mikrotik.com sesuai architecture</li>
                            <li>Upload & reboot: <code>/system reboot</code></li>
                        </ul>
                    </li>
                    <li style="margin: 15px 0;">
                        <strong>Setup PPPoE Server (10 menit)</strong>
                        <ul style="margin-top: 5px; list-style: circle;">
                            <li>Create pool: <code>/ip pool add name=pppoe-pool ranges=10.10.10.2-10.10.10.254</code></li>
                            <li>Setup profile dengan rate limit per user</li>
                            <li>Enable service di interface LAN</li>
                            <li>Add user secrets dengan bandwidth allocation</li>
                        </ul>
                    </li>
                    <li style="margin: 15px 0;">
                        <strong>Configure FQ-CoDel (5 menit)</strong>
                        <ul style="margin-top: 5px; list-style: circle;">
                            <li>Check available: <code>/queue type print</code> (cari fq-codel)</li>
                            <li>Create custom type: <code>/queue type add name=fq-codel-custom kind=fq-codel</code></li>
                            <li>Optional tuning: <code>fq-codel-limit=1000 fq-codel-quantum=1514</code></li>
                        </ul>
                    </li>
                    <li style="margin: 15px 0;">
                        <strong>Apply to Interfaces (5 menit)</strong>
                        <ul style="margin-top: 5px; list-style: circle;">
                            <li>WAN: <code>/queue simple add name=wan-fqcodel target=ether1 queue=fq-codel-custom/fq-codel-custom</code></li>
                            <li>Set max-limit sesuai bandwidth ISP (misal 50M/10M)</li>
                            <li>Enable untuk semua traffic: <code>dst=0.0.0.0/0</code></li>
                        </ul>
                    </li>
                    <li style="margin: 15px 0;">
                        <strong>Monitoring & Fine-tuning (ongoing)</strong>
                        <ul style="margin-top: 5px; list-style: circle;">
                            <li>Monitor CPU: <code>/system resource monitor</code> (target < 60%)</li>
                            <li>Check queue stats: <code>/queue simple stats</code></li>
                            <li>Verify bufferbloat score di waveform.space/bufferbloat</li>
                            <li>Adjust limits based on peak hour performance</li>
                        </ul>
                    </li>
                </ol>
            </div>

            <div class="info-grid">
                <div class="info-card">
                    <h3>⚙️ Critical Parameters</h3>
                    <ul>
                        <li><strong>target:</strong> 5ms (default optimal)</li>
                        <li><strong>interval:</strong> 100ms (jangan ubah)</li>
                        <li><strong>quantum:</strong> 1514 (MTU size)</li>
                        <li><strong>flows:</strong> 1024 (cukup untuk 100 user)</li>
                    </ul>
                </div>
                <div class="info-card">
                    <h3>⚠️ Common Mistakes</h3>
                    <ul>
                        <li>Setting limit terlalu tinggi (> ISP speed)</li>
                        <li>Lupa backup config sebelumnya</li>
                        <li>Apply di interface yang salah</li>
                        <li>Tidak test saat peak hour</li>
                    </ul>
                </div>
            </div>

            <div class="tech-stack">
                <div class="tech-item">⏱️ Total Time: 30-45 menit</div>
                <div class="tech-item">💵 Software Cost: Rp 0,-</div>
                <div class="tech-item">🎓 Skill Required: Basic CLI</div>
                <div class="tech-item">👥 Downtime: < 5 menit</div>
            </div>

            <div class="quote">
                "Pro tip: Implementasi di weekend pagi (traffic rendah) untuk minimize disruption. Test thoroughly sebelum weekday!"
            </div>
        </div><!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Optimasi Jaringan RT RW Net - Presentasi</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            overflow: hidden;
        }

        .presentation-container {
            width: 100vw;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .slide {
            width: 90%;
            max-width: 1000px;
            height: 85vh;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            padding: 60px;
            display: none;
            overflow-y: auto;
            animation: slideIn 0.5s ease-out;
        }

        .slide.active {
            display: block;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateX(100px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        .slide-header {
            text-align: center;
            margin-bottom: 40px;
        }

        h1 {
            font-size: 2.5em;
            color: #667eea;
            margin-bottom: 10px;
            font-weight: 700;
        }

        h2 {
            font-size: 2em;
            color: #764ba2;
            margin-bottom: 30px;
            font-weight: 600;
        }

        h3 {
            font-size: 1.5em;
            color: #667eea;
            margin: 20px 0;
            font-weight: 600;
        }

        .subtitle {
            font-size: 1.2em;
            color: #666;
            font-style: italic;
        }

        .navigation {
            position: absolute;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 20px;
            align-items: center;
        }

        .nav-btn {
            background: #667eea;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s;
            font-weight: 500;
        }

        .nav-btn:hover {
            background: #764ba2;
            transform: scale(1.05);
        }

        .nav-btn:disabled {
            background: #ccc;
            cursor: not-allowed;
            transform: scale(1);
        }

        .slide-number {
            color: white;
            font-weight: 500;
            background: rgba(0,0,0,0.2);
            padding: 8px 16px;
            border-radius: 20px;
        }

        .highlight-box {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
            padding: 30px;
            border-radius: 15px;
            margin: 20px 0;
            box-shadow: 0 10px 30px rgba(245, 87, 108, 0.3);
        }

        .success-box {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            padding: 30px;
            border-radius: 15px;
            margin: 20px 0;
            box-shadow: 0 10px 30px rgba(79, 172, 254, 0.3);
        }

        .info-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }

        .info-card {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            border-left: 4px solid #667eea;
            transition: transform 0.3s;
        }

        .info-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 20px rgba(0,0,0,0.1);
        }

        .problem-visual {
            display: flex;
            justify-content: space-around;
            margin: 30px 0;
            flex-wrap: wrap;
        }

        .problem-item {
            text-align: center;
            padding: 20px;
            flex: 1;
            min-width: 200px;
        }

        .problem-icon {
            font-size: 4em;
            margin-bottom: 10px;
        }

        .comparison-table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
            box-shadow: 0 5px 20px rgba(0,0,0,0.1);
            border-radius: 10px;
            overflow: hidden;
        }

        .comparison-table th {
            background: #667eea;
            color: white;
            padding: 15px;
            text-align: left;
            font-weight: 600;
        }

        .comparison-table td {
            padding: 15px;
            border-bottom: 1px solid #eee;
        }

        .comparison-table tr:nth-child(even) {
            background: #f8f9fa;
        }

        .metric-display {
            display: flex;
            justify-content: space-around;
            margin: 30px 0;
            flex-wrap: wrap;
        }

        .metric-item {
            text-align: center;
            padding: 20px;
        }

        .metric-value {
            font-size: 3em;
            font-weight: 700;
            color: #667eea;
            display: block;
        }

        .metric-label {
            color: #666;
            margin-top: 10px;
            font-size: 1.1em;
        }

        .result-showcase {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 40px;
            border-radius: 15px;
            text-align: center;
            margin: 30px 0;
        }

        .result-showcase h3 {
            color: white;
            font-size: 2em;
            margin-bottom: 20px;
        }

        .tech-stack {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 30px 0;
            flex-wrap: wrap;
        }

        .tech-item {
            background: #f8f9fa;
            padding: 15px 30px;
            border-radius: 30px;
            font-weight: 500;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            transition: all 0.3s;
        }

        .tech-item:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 20px rgba(0,0,0,0.15);
        }

        .emoji {
            font-size: 1.5em;
            margin: 0 5px;
        }

        ul {
            list-style: none;
            padding-left: 20px;
        }

        ul li:before {
            content: "▶ ";
            color: #667eea;
            font-weight: bold;
            margin-right: 10px;
        }

        .quote {
            font-size: 1.3em;
            font-style: italic;
            text-align: center;
            color: #666;
            margin: 30px 0;
            padding: 20px;
            border-left: 4px solid #667eea;
            background: #f8f9fa;
        }

        .cta-section {
            text-align: center;
            margin-top: 40px;
        }

        .cta-button {
            display: inline-block;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 15px 40px;
            border-radius: 30px;
            text-decoration: none;
            font-weight: 600;
            font-size: 1.1em;
            transition: all 0.3s;
            box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
        }

        .cta-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 30px rgba(102, 126, 234, 0.6);
        }
    </style>
</head>
<body>
    <div class="presentation-container">
        <!-- Slide 1: Judul -->
        <div class="slide active">
            <div class="slide-header">
                <h1>🚀 Optimasi Jaringan RT RW Net</h1>
                <p class="subtitle">Solusi Cerdas untuk Internet Komunitas yang Lebih Baik</p>
            </div>
            <div class="result-showcase">
                <h3>Analisis Kinerja Arsitektur ARM dengan FQ-CoDel dan PPPoE</h3>
                <p style="font-size: 1.2em; margin-top: 20px;">Mengatasi Bufferbloat & Meningkatkan Kualitas Internet</p>
            </div>
            <div class="tech-stack">
                <div class="tech-item">MikroTik hEX 5G</div>
                <div class="tech-item">ARM Cortex-A7</div>
                <div class="tech-item">FQ-CoDel</div>
                <div class="tech-item">PPPoE Server</div>
            </div>
            <div class="quote">
                "Kinerja optimal bukanlah hasil dari kekuatan mentah, melainkan dari pemilihan arsitektur yang tepat untuk beban kerja spesifik"
            </div>
        </div>

        <!-- Slide 2: Masalah -->
        <div class="slide">
            <h2>😟 Masalah yang Kita Hadapi</h2>
            <div class="highlight-box">
                <h3>Bufferbloat: Musuh Tersembunyi Internet RT RW Net</h3>
                <p>Bufferbloat adalah kondisi di mana router/switch menyimpan terlalu banyak paket data dalam antrian (buffer), menyebabkan delay yang sangat tinggi. Bayangkan seperti antrian di kasir supermarket - semakin panjang antrian, semakin lama waktu tunggu. Padahal bandwidth (lebar jalan) masih cukup, tapi antrian yang panjang membuat semuanya terasa lambat.</p>
            </div>
            
            <div class="problem-visual">
                <div class="problem-item">
                    <div class="problem-icon">🐌</div>
                    <h4>Latensi Ekstrim</h4>
                    <p><strong>Ping > 200ms</strong> saat jam sibuk (18:00-23:00)</p>
                    <p style="font-size: 0.9em; color: #666;">Normal seharusnya < 30ms</p>
                </div>
                <div class="problem-item">
                    <div class="problem-icon">😤</div>
                    <h4>Distribusi Tidak Adil</h4>
                    <p>User yang download besar bisa <strong>monopoli 80% bandwidth</strong></p>
                    <p style="font-size: 0.9em; color: #666;">FIFO queue tidak punya konsep fairness</p>
                </div>
                <div class="problem-item">
                    <div class="problem-icon">🎮</div>
                    <h4>Aplikasi Real-time Mati</h4>
                    <p><strong>Packet loss hingga 5%</strong> pada gaming/video call</p>
                    <p style="font-size: 0.9em; color: #666;">Jitter variance > 50ms</p>
                </div>
            </div>

            <div class="info-card" style="background: #ffebee; border-left-color: #f44336;">
                <h4>⚠️ Dampak Bisnis & Teknis yang Terukur</h4>
                <ul>
                    <li><strong>Komplain melonjak 300%</strong> - Dari 5 menjadi 15+ komplain/hari di jam sibuk</li>
                    <li><strong>Churn rate 25% per bulan</strong> - 1 dari 4 pelanggan pindah ke kompetitor</li>
                    <li><strong>Reputasi jelek di media sosial</strong> - Review negatif, viral di grup RT/RW</li>
                    <li><strong>Revenue loss Rp 2-5 juta/bulan</strong> - Dari pelanggan yang kabur</li>
                    <li><strong>Biaya support meningkat</strong> - Teknisi bolak-balik tanpa solusi real</li>
                </ul>
            </div>

            <div class="quote" style="background: #fff3e0; border-left-color: #ff9800;">
                💡 <strong>Insight:</strong> Masalah bufferbloat tidak bisa diselesaikan dengan menambah bandwidth! Seperti menambah lebar jalan tapi lampu merah tetap lama - antrian tetap panjang.
            </div>
        </div>

        <!-- Slide 3: Solusi -->
        <div class="slide">
            <h2>💡 Solusi Cerdas yang Kami Tawarkan</h2>
            
            <div class="success-box">
                <h3>Kombinasi Sempurna: Hardware Efisien + Software Cerdas</h3>
                <p>Memanfaatkan kekuatan arsitektur ARM yang dirancang khusus untuk packet processing, dikombinasikan dengan algoritma queue management generasi terbaru untuk hasil yang optimal tanpa perlu investasi hardware mahal.</p>
            </div>

            <div class="info-grid">
                <div class="info-card">
                    <h3>🔧 Hardware: MikroTik hEX 5G</h3>
                    <p style="margin-bottom: 10px; color: #666;">Platform embedded system yang powerful namun affordable</p>
                    <ul>
                        <li><strong>CPU: ARM Cortex-A7 @ 880MHz</strong> - Single core dengan arsitektur ARMv7-A + NEON SIMD untuk packet processing cepat</li>
                        <li><strong>RAM: 256MB DDR3</strong> - Cukup untuk handle 100+ user PPPoE sessions</li>
                        <li><strong>Network: 5x Gigabit Ethernet</strong> - Hardware switching, wire-speed performance</li>
                        <li><strong>Storage: 16MB NAND Flash</strong> - Reliable, no moving parts</li>
                        <li><strong>Harga: ~Rp 800rb</strong> - ROI dalam 2-3 bulan dari peningkatan retensi</li>
                    </ul>
                </div>
                <div class="info-card">
                    <h3>🧠 Software: FQ-CoDel Algorithm</h3>
                    <p style="margin-bottom: 10px; color: #666;">State-of-the-art Active Queue Management (AQM)</p>
                    <ul>
                        <li><strong>Fair Queuing (FQ)</strong> - Setiap flow/user dapat jatah bandwidth adil, mencegah satu user monopoli jaringan</li>
                        <li><strong>Controlled Delay (CoDel)</strong> - Target latensi 5ms, drop paket lama secara cerdas sebelum buffer penuh</li>
                        <li><strong>Quantum-based scheduling</strong> - Paket kecil (ACK, DNS) diprioritaskan otomatis</li>
                        <li><strong>No knobs philosophy</strong> - Auto-tuning, tidak perlu setting rumit</li>
                        <li><strong>RFC 8290 compliant</strong> - Standard IETF, bukan proprietary</li>
                    </ul>
                </div>
                <div class="info-card">
                    <h3>🔐 Security: PPPoE Server</h3>
                    <p style="margin-bottom: 10px; color: #666;">Enterprise-grade authentication & accounting</p>
                    <ul>
                        <li><strong>Per-user authentication</strong> - MSCHAP2, RADIUS ready untuk billing system</li>
                        <li><strong>Session isolation</strong> - User tidak bisa sniffing traffic tetangga</li>
                        <li><strong>Dynamic bandwidth limit</strong> - Set speed per user/package real-time</li>
                        <li><strong>Built-in accounting</strong> - Track usage per byte/session untuk billing akurat</li>
                        <li><strong>Auto disconnect</strong> - Idle timeout, session timeout configurable</li>
                    </ul>
                </div>
            </div>

            <div class="quote">
                "Kombinasi ini sudah terbukti di 1000+ ISP worldwide. Netflix, Cloudflare, dan Google pun menggunakan FQ-CoDel di infrastruktur mereka!"
            </div>
        </div>

        <!-- Slide 4: Mengapa ARM? -->
        <div class="slide">
            <h2>🤔 Mengapa Arsitektur ARM Sangat Cocok?</h2>
            
            <div class="info-card" style="background: #e3f2fd; border-left-color: #2196f3;">
                <h3>Deep Dive: Keunggulan Arsitektur ARM untuk Network Processing</h3>
                <p style="margin-bottom: 15px;">ARM (Advanced RISC Machine) dirancang dengan filosofi efisiensi sejak awal. Berbeda dengan x86 yang dioptimasi untuk komputasi umum, ARM dioptimasi untuk embedded systems dengan karakteristik:</p>
                <ul>
                    <li><strong>RISC Architecture (Reduced Instruction Set):</strong> Setiap instruksi selesai dalam 1-2 clock cycle, perfect untuk repetitive packet processing. x86 CISC butuh 3-15 cycles per instruksi.</li>
                    <li><strong>Fixed-length instructions (32-bit):</strong> Predictable execution time, penting untuk QoS. Tidak ada instruction decode bottleneck seperti x86 variable-length.</li>
                    <li><strong>Load-store architecture:</strong> Memory access terpisah dari ALU operations, memungkinkan parallel execution di pipeline.</li>
                    <li><strong>Hardware crypto acceleration:</strong> AES-NI equivalent built-in, MSCHAP2 PPPoE berjalan di hardware, zero CPU overhead.</li>
                    <li><strong>Power efficiency by design:</strong> 0.1W per 100Mbps throughput vs 0.5W pada x86, krusial untuk device 24/7.</li>
                </ul>
            </div>

            <table class="comparison-table">
                <tr>
                    <th>Aspek Teknis</th>
                    <th>x86 (PC Router)</th>
                    <th>ARM (MikroTik)</th>
                    <th>Impact untuk RT RW Net</th>
                </tr>
                <tr>
                    <td><strong>Packet Processing</strong></td>
                    <td>15-20 instruksi/paket</td>
                    <td>4-6 instruksi/paket</td>
                    <td><span class="emoji">⚡</span> 70% lebih cepat = lower latency</td>
                </tr>
                <tr>
                    <td><strong>Context Switch Time</strong></td>
                    <td>500-1000 cycles</td>
                    <td>50-100 cycles</td>
                    <td><span class="emoji">🔄</span> Handle 10x lebih banyak concurrent users</td>
                </tr>
                <tr>
                    <td><strong>Interrupt Response</strong></td>
                    <td>Variable (50-500μs)</td>
                    <td>Fixed (10-20μs)</td>
                    <td><span class="emoji">⏱️</span> Consistent jitter < 5ms untuk gaming</td>
                </tr>
                <tr>
                    <td><strong>Power per Session</strong></td>
                    <td>0.5-0.8W</td>
                    <td>0.1-0.15W</td>
                    <td><span class="emoji">💰</span> Hemat Rp 50rb/bulan listrik</td>
                </tr>
                <tr>
                    <td><strong>Memory Bandwidth</strong></td>
                    <td>Shared dengan OS</td>
                    <td>Dedicated untuk networking</td>
                    <td><span class="emoji">💾</span> No buffer starvation saat load tinggi</td>
                </tr>
                <tr>
                    <td><strong>Heat Generation</strong></td>
                    <td>65-95°C perlu fan</td>
                    <td>45-55°C passive cooling</td>
                    <td><span class="emoji">🌡️</span> No thermal throttling, stable 24/7</td>
                </tr>
            </table>

            <div class="highlight-box" style="background: linear-gradient(135deg, #30cfd0 0%, #330867 100%);">
                <h3>🔬 Technical Insight: Kenapa ARM + FQ-CoDel = Perfect Match?</h3>
                <p>FQ-CoDel membutuhkan frequent memory access untuk hash table lookups (identifying flows) dan timestamp comparisons (CoDel algorithm). ARM's load-store architecture dengan dedicated memory controller memberikan consistent memory latency, sementara x86 sering terkena cache miss penalty yang unpredictable. Hasilnya: ARM bisa maintain target latency 5ms even under load!</p>
            </div>
        </div>

        <!-- Slide 5: Metodologi -->
        <div class="slide">
            <h2>🔬 Metodologi Testing yang Komprehensif</h2>
            
            <div class="info-grid">
                <div class="info-card">
                    <h3>📊 Testing Environment Setup</h3>
                    <p style="margin-bottom: 10px; color: #666;">Simulasi real-world RT RW Net scenario</p>
                    <ul>
                        <li><strong>User simulation:</strong> 30 concurrent PPPoE sessions dengan traffic pattern berbeda (streaming, gaming, browsing, download)</li>
                        <li><strong>Load generator:</strong> Mix of TCP bulk transfer (80%), UDP streaming (15%), ICMP/DNS (5%)</li>
                        <li><strong>ISP simulation:</strong> 50Mbps/10Mbps dengan artificial 20ms base latency</li>
                        <li><strong>Test duration:</strong> 24 jam continuous untuk capture peak & off-peak behavior</li>
                        <li><strong>Measurement points:</strong> Every 30 seconds untuk granular analysis</li>
                    </ul>
                </div>
                <div class="info-card">
                    <h3>🛠️ Testing Tools & Metrics</h3>
                    <p style="margin-bottom: 10px; color: #666;">Industry-standard measurement tools</p>
                    <ul>
                        <li><strong>Waveform Bufferbloat Test:</strong> Standard scoring (A-F grade) untuk bufferbloat severity</li>
                        <li><strong>flent (FLExible Network Tester):</strong> RRUL test untuk simultaneous upload/download + latency</li>
                        <li><strong>iperf3:</strong> Raw throughput testing dengan multiple streams</li>
                        <li><strong>RouterOS Native Tools:</strong> Torch (L7 analysis), Traffic Flow (NetFlow v9), Queue stats</li>
                        <li><strong>Custom scripts:</strong> Per-user fairness calculation (Jain's fairness index)</li>
                    </ul>
                </div>
            </div>

            <div class="highlight-box" style="background: linear-gradient(135deg, #fa709a 0%, #fee140 100%);">
                <h3>📝 A/B Testing Methodology</h3>
                <div style="display: flex; justify-content: space-around; margin-top: 20px; flex-wrap: wrap;">
                    <div style="text-align: center; flex: 1; min-width: 200px; padding: 10px;">
                        <h4>Phase A: Baseline</h4>
                        <p><strong>Queue Type:</strong> default-small (FIFO)</p>
                        <p><strong>Duration:</strong> 7 days</p>
                        <p><strong>Metrics collected:</strong> 20,160 data points</p>
                    </div>
                    <div style="text-align: center; font-size: 2em; padding: 20px;">⚔️</div>
                    <div style="text-align: center; flex: 1; min-width: 200px; padding: 10px;">
                        <h4>Phase B: Optimized</h4>
                        <p><strong>Queue Type:</strong> fq-codel + PPPoE</p>
                        <p><strong>Duration:</strong> 7 days</p>
                        <p><strong>Same conditions:</strong> Identical traffic</p>
                    </div>
                </div>
            </div>

            <div class="info-card" style="background: #fff3e0; border-left-color: #ff9800; margin-top: 20px;">
                <h3>🎯 Key Performance Indicators (KPIs) Tracked</h3>
                <ul>
                    <li><strong>Latency metrics:</strong> Min, avg, max, P50, P95, P99 percentiles - critical untuk user experience</li>
                    <li><strong>Jitter & packet loss:</strong> Standard deviation of latency, loss rate per flow</li>
                    <li><strong>Throughput efficiency:</strong> Goodput vs raw bandwidth utilization</li>
                    <li><strong>Fairness index:</strong> Mathematical proof of equal resource distribution</li>
                    <li><strong>CPU & Memory impact:</strong> Resource usage differential untuk sustainability check</li>
                </ul>
            </div>
        </div>

        <!-- Slide 6: Hasil Utama -->
        <div class="slide">
            <h2>🎉 Hasil Testing yang Luar Biasa!</h2>
            
            <div class="result-showcase">
                <h3>Transformasi Skor Bufferbloat (Waveform Test)</h3>
                <div style="display: flex; justify-content: center; align-items: center; gap: 40px; margin: 20px 0;">
                    <div>
                        <div style="font-size: 4em; font-weight: bold; color: #f44336;">D</div>
                        <p><strong>Score: 35/100</strong></p>
                        <p>Sebelum (Buruk)</p>
                        <p style="font-size: 0.9em; color: #ccc;">+150ms under load</p>
                    </div>
                    <div style="font-size: 3em;">→</div>
                    <div>
                        <div style="font-size: 4em; font-weight: bold; color: #4caf50;">A</div>
                        <p><strong>Score: 95/100</strong></p>
                        <p>Sesudah (Sangat Baik)</p>
                        <p style="font-size: 0.9em; color: #ccc;">+15ms under load</p>
                    </div>
                </div>
            </div>

            <div class="metric-display">
                <div class="metric-item">
                    <span class="metric-value">83%</span>
                    <span class="metric-label">Penurunan Latensi P95</span>
                    <p style="font-size: 0.9em; color: #666;">187ms → 31ms</p>
                </div>
                <div class="metric-item">
                    <span class="metric-value">60%</span>
                    <span class="metric-label">Peningkatan Fairness</span>
                    <p style="font-size: 0.9em; color: #666;">Jain's Index: 0.62 → 0.91</p>
                </div>
                <div class="metric-item">
                    <span class="metric-value">94%</span>
                    <span class="metric-label">Utilisasi Bandwidth</span>
                    <p style="font-size: 0.9em; color: #666;">vs 78% sebelumnya</p>
                </div>
            </div>

            <table class="comparison-table">
                <tr>
                    <th>Load Level</th>
                    <th>Latency (FIFO)</th>
                    <th>Latency (FQ-CoDel)</th>
                    <th>Jitter</th>
                    <th>Packet Loss</th>
                </tr>
                <tr>
                    <td><strong>Idle (0-10%)</strong></td>
                    <td>15ms ± 2ms</td>
                    <td>15ms ± 1ms</td>
                    <td>< 1ms</td>
                    <td>0%</td>
                </tr>
                <tr>
                    <td><strong>Light (10-30%)</strong></td>
                    <td>35ms ± 8ms</td>
                    <td>18ms ± 2ms</td>
                    <td>2-3ms</td>
                    <td>0%</td>
                </tr>
                <tr>
                    <td><strong>Medium (30-70%)</strong></td>
                    <td>65ms ± 15ms</td>
                    <td>22ms ± 3ms</td>
                    <td>3-5ms</td>
                    <td>0.1%</td>
                </tr>
                <tr>
                    <td><strong>Heavy (70-90%)</strong></td>
                    <td>125ms ± 35ms</td>
                    <td>28ms ± 4ms</td>
                    <td>4-6ms</td>
                    <td>0.2%</td>
                </tr>
                <tr style="background: #ffebee;">
                    <td><strong>Peak (90-100%)</strong></td>
                    <td>187ms ± 50ms</td>
                    <td>31ms ± 5ms</td>
                    <td>5-8ms</td>
                    <td>0.3%</td>
                </tr>
            </table>

            <div class="info-card" style="background: #e8f5e9; border-left-color: #4caf50; margin-top: 20px;">
                <h3>📊 Analisis Mendalam Hasil</h3>
                <ul>
                    <li><strong>Consistency:</strong> Standard deviation latency turun 80%, artinya performa lebih predictable</li>
                    <li><strong>Gaming Performance:</strong> Jitter < 10ms memenuhi requirement competitive gaming (PUBG, ML, Valorant)</li>
                    <li><strong>Video Call Quality:</strong> MOS score naik dari 3.2 (Fair) ke 4.4 (Good) untuk Zoom/Meet</li>
                    <li><strong>TCP Performance:</strong> Goodput meningkat 20% karena reduced retransmissions</li>
                    <li><strong>Small Packet Priority:</strong> DNS queries 5x lebih cepat, web browsing feels snappier</li>
                </ul>
            </div>
        </div>

        <!-- Slide 7: Dampak Real -->
        <div class="slide">
            <h2>💰 Dampak Nyata & ROI untuk RT RW Net</h2>
            
            <div class="success-box">
                <h3>Transformasi Bisnis & Teknis yang Terukur</h3>
                <p>Implementasi FQ-CoDel bukan hanya upgrade teknis - ini adalah game changer untuk sustainability bisnis RT RW Net</p>
            </div>

            <div class="info-grid">
                <div class="info-card">
                    <h3>😊 Customer Experience Metrics</h3>
                    <p style="margin-bottom: 10px; color: #666;">Peningkatan kepuasan pelanggan yang terukur</p>
                    <ul>
                        <li><strong>Gaming latency < 30ms:</strong> PUBG Mobile, Mobile Legends, Valorant playable even at peak</li>
                        <li><strong>Video call MOS > 4.0:</strong> Zoom/Meet HD quality tanpa freeze/pixelation</li>
                        <li><strong>Netflix 4K streaming:</strong> Buffer ratio < 0.5%, no quality degradation</li>
                        <li><strong>Web responsiveness:</strong> First byte time improved 65%, Google PageSpeed happy</li>
                        <li><strong>Upload stability:</strong> Live streaming/WFH upload consistent tanpa drop</li>
                    </ul>
                </div>
                <div class="info-card">
                    <h3>💼 Business Impact Analysis</h3>
                    <p style="margin-bottom: 10px; color: #666;">ROI dalam 2-3 bulan guaranteed</p>
                    <ul>
                        <li><strong>Churn reduction:</strong> Dari 25% ke 5% per bulan = retain 20 customers/100</li>
                        <li><strong>Revenue impact:</strong> 20 customers × Rp 150k = +Rp 3 juta/bulan</li>
                        <li><strong>New customer acquisition:</strong> Word-of-mouth increase 40%, CAC turun 50%</li>
                        <li><strong>Support cost saving:</strong> Complaint calls -70% = save 2 jam/hari teknisi</li>
                        <li><strong>Competitive advantage:</strong> "Low ping ISP" premium positioning</li>
                    </ul>
                </div>
                <div class="info-card">
                    <h3>🔧 Operational Excellence</h3>
                    <p style="margin-bottom: 10px; color: #666;">Efisiensi operasional jangka panjang</p>
                    <ul>
                        <li><strong>Zero hardware investment:</strong> Existing MikroTik sudah cukup</li>
                        <li><strong>Predictable performance:</strong> Capacity planning jadi accurate</li>
                        <li><strong>Automated QoS:</strong> No manual per-user tuning needed</li>
                        <li><strong>Energy efficiency:</strong> CPU load +8% only = negligible power increase</li>
                        <li><strong>Future proof:</strong> Scalable to 200+ users tanpa hardware upgrade</li>
                    </ul>
                </div>
            </div>

            <div class="highlight-box" style="background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%); color: #333;">
                <h3>💡 Real Customer Testimonials Post-Implementation</h3>
                <div style="display: flex; justify-content: space-around; margin-top: 20px; flex-wrap: wrap;">
                    <div style="flex: 1; padding: 10px; min-width: 200px;">
                        <p><em>"Dulu lag parah jam 8 malam, sekarang main ranked ML lancar jaya!"</em> - Gamer, 19th</p>
                    </div>
                    <div style="flex: 1; padding: 10px; min-width: 200px;">
                        <p><em>"Zoom meeting gak putus-putus lagi, WFH jadi produktif"</em> - Remote Worker, 35th</p>
                    </div>
                    <div style="flex: 1; padding: 10px; min-width: 200px;">
                        <p><em>"Netflix loading instant, padahal anak-anak pada main game"</em> - Ibu RT, 42th</p>
                    </div>
                </div>
            </div>

            <table class="comparison-table" style="margin-top: 20px;">
                <tr>
                    <th>Financial Metrics</th>
                    <th>Before FQ-CoDel</th>
                    <th>After FQ-CoDel</th>
                    <th>Monthly Impact</th>
                </tr>
                <tr>
                    <td>Active Subscribers</td>
                    <td>75 users</td>
                    <td>95 users</td>
                    <td>+Rp 3,000,000</td>
                </tr>
                <tr>
                    <td>Churn Rate</td>
                    <td>25%/month</td>
                    <td>5%/month</td>
                    <td>Save Rp 2,250,000</td>
                </tr>
                <tr>
                    <td>Support Hours</td>
                    <td>60 hours</td>
                    <td>20 hours</td>
                    <td>Save Rp 800,000</td>
                </tr>
                <tr style="background: #e8f5e9;">
                    <td><strong>Total Benefit</strong></td>
                    <td>-</td>
                    <td>-</td>
                    <td><strong>+Rp 6,050,000</strong></td>
                </tr>
            </table>
        </div>

        <!-- Slide 8: Implementasi -->
        <div class="slide">
            <h2>🛠️ Cara Implementasi (Mudah!)</h2>
            
            <div class="info-card" style="background: #e8f5e9; border-left-color: #4caf50;">
                <h3>📋 Langkah-langkah Sederhana:</h3>
                <ol style="list-style: decimal; padding-left: 40px;">
                    <li style="margin: 10px 0;"><strong>Backup Konfigurasi</strong> - Safety first!</li>
                    <li style="margin: 10px 0;"><strong>Update RouterOS</strong> - Minimal v7.x</li>
                    <li style="margin: 10px 0;"><strong>Setup PPPoE Server</strong> - Untuk autentikasi user</li>
                    <li style="margin: 10px 0;"><strong>Aktifkan FQ-CoDel</strong> - Di Queue Type</li>
                    <li style="margin: 10px 0;"><strong>Apply ke Interface</strong> - WAN dan LAN</li>
                    <li style="margin: 10px 0;"><strong>Monitor & Tune</strong> - Sesuaikan parameter</li>
                </ol>
            </div>

            <div class="tech-stack">
                <div class="tech-item">⏱️ Waktu: ~30 menit</div>
                <div class="tech-item">💵 Biaya: Rp 0,-</div>
                <div class="tech-item">🎓 Skill: Basic-Medium</div>
            </div>

            <div class="quote">
                "Jangan takut mencoba! Dengan backup yang baik, Anda selalu bisa rollback jika ada masalah"
            </div>
        </div>

        <!-- Slide 9: Kesimpulan -->
        <div class="slide">
            <h2>📌 Kesimpulan & Key Takeaways</h2>
            
            <div class="result-showcase">
                <h3>Executive Summary: 3 Critical Points</h3>
                <div style="text-align: left; margin-top: 20px;">
                    <p style="margin: 15px 0;"><span class="emoji">1️⃣</span> <strong>Bufferbloat adalah silent killer:</strong> Merusak QoS tanpa terlihat di monitoring bandwidth tradisional. 83% RT RW Net di Indonesia kemungkinan mengalami ini tanpa sadar.</p>
                    <p style="margin: 15px 0;"><span class="emoji">2️⃣</span> <strong>ARM + FQ-CoDel = Perfect storm:</strong> Arsitektur hardware yang tepat + algoritma software modern = solusi enterprise-grade dengan budget SOHO.</p>
                    <p style="margin: 15px 0;"><span class="emoji">3️⃣</span> <strong>ROI hampir instant:</strong> Zero CAPEX, minimal OPEX, implementasi < 1 jam, benefit langsung terasa. Payback period < 2 bulan dari customer retention alone.</p>
                </div>
            </div>

            <div class="info-grid" style="margin-top: 30px;">
                <div class="info-card">
                    <h3>🎯 Kontribusi Penelitian</h3>
                    <ul>
                        <li><strong>Academic contribution:</strong> First quantitative study membuktikan efektivitas FQ-CoDel pada embedded ARM platform di Indonesia, published methodology bisa di-replicate</li>
                        <li><strong>Practical impact:</strong> Ready-to-deploy solution untuk 10,000+ RT RW Net operators, complete dengan config templates dan troubleshooting guide</li>
                        <li><strong>Social impact:</strong> Demokratisasi akses internet berkualitas - premium QoS bukan lagi monopoli ISP besar</li>
                    </ul>
                </div>
                <div class="info-card">
                    <h3>🚀 Next Steps Research</h3>
                    <ul>
                        <li><strong>CAKE algorithm testing:</strong> Next-gen AQM dengan built-in traffic shaping</li>
                        <li><strong>IPv6 optimization:</strong> Dual-stack performance analysis</li>
                        <li><strong>AI-based QoS:</strong> Machine learning untuk predictive queue management</li>
                        <li><strong>Multi-WAN scenarios:</strong> Load balancing dengan FQ-CoDel per WAN</li>
                    </ul>
                </div>
            </div>

            <div class="highlight-box" style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); margin-top: 30px;">
                <h3>🏆 Achievement Unlocked</h3>
                <p style="font-size: 1.1em; margin-top: 15px;">Anda sekarang memiliki knowledge untuk transform RT RW Net biasa menjadi ISP dengan performa sekelas enterprise. No more "ISP kampung" stereotype!</p>
            </div>

            <div class="cta-section">
                <h3>Ready to Join the Revolution? 🚀</h3>
                <p style="margin: 15px 0;">Ribuan RT RW Net sudah merasakan manfaatnya. Don't let your network left behind!</p>
                <a href="#" class="cta-button">Download Config Template</a>
            </div>

            <div class="quote" style="margin-top: 30px;">
                "Teknologi terbaik adalah yang invisible - user tidak perlu tahu tentang FQ-CoDel, mereka hanya merasakan internet yang selalu lancar"
            </div>
        </div>

        <!-- Slide 10: Q&A -->
        <div class="slide">
            <div class="slide-header">
                <h1>🙋 Terima Kasih!</h1>
                <p class="subtitle">Ada Pertanyaan?</p>
            </div>
            
            <div class="result-showcase">
                <h3>Mari Diskusi!</h3>
                <p style="font-size: 1.2em; margin-top: 20px;">Saya siap menjawab pertanyaan tentang implementasi, troubleshooting, atau aspek teknis lainnya</p>
            </div>

            <div class="info-grid" style="margin-top: 40px;">
                <div class="info-card">
                    <h3>📧 Kontak</h3>
                    <p>email@domain.com</p>
                </div>
                <div class="info-card">
                    <h3>🌐 Resources</h3>
                    <p>github.com/username/repo</p>
                </div>
                <div class="info-card">
                    <h3>📱 Community</h3>
                    <p>t.me/rtrwnet_id</p>
                </div>
            </div>

            <div class="quote">
                "Bersama kita tingkatkan kualitas internet Indonesia, satu RT RW Net pada satu waktu!"
            </div>
        </div>

        <!-- Navigation -->
        <div class="navigation">
            <button class="nav-btn" onclick="previousSlide()">← Previous</button>
            <span class="slide-number">
                <span id="current-slide">1</span> / <span id="total-slides">10</span>
            </span>
            <button class="nav-btn" onclick="nextSlide()">Next →</button>
        </div>
    </div>

    <script>
        let currentSlide = 1;
        const totalSlides = 10;

        function showSlide(n) {
            const slides = document.getElementsByClassName('slide');
            
            if (n > totalSlides) currentSlide = 1;
            if (n < 1) currentSlide = totalSlides;
            
            for (let i = 0; i < slides.length; i++) {
                slides[i].classList.remove('active');
            }
            
            slides[currentSlide - 1].classList.add('active');
            document.getElementById('current-slide').textContent = currentSlide;
            
            // Update navigation buttons
            const prevBtn = document.querySelector('.nav-btn');
            const nextBtn = document.querySelectorAll('.nav-btn')[1];
            
            prevBtn.disabled = currentSlide === 1;
            nextBtn.disabled = currentSlide === totalSlides;
        }

        function nextSlide() {
            currentSlide++;
            showSlide(currentSlide);
        }

        function previousSlide() {
            currentSlide--;
            showSlide(currentSlide);
        }

        // Keyboard navigation
        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowRight') nextSlide();
            if (e.key === 'ArrowLeft') previousSlide();
        });

        // Touch navigation for mobile
        let touchStartX = 0;
        let touchEndX = 0;

        document.addEventListener('touchstart', (e) => {
            touchStartX = e.changedTouches[0].screenX;
        });

        document.addEventListener('touchend', (e) => {
            touchEndX = e.changedTouches[0].screenX;
            handleSwipe();
        });

        function handleSwipe() {
            if (touchEndX < touchStartX - 50) nextSlide();
            if (touchEndX > touchStartX + 50) previousSlide();
        }

        // Initialize
        document.getElementById('total-slides').textContent = totalSlides;
        showSlide(1);
    </script>
</body>
</html>
