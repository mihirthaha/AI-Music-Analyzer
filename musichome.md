<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Music Analyzer</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: white;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
        }

        .header {
            text-align: center;
            margin-bottom: 3rem;
        }

        .header h1 {
            font-size: 3rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
            background: linear-gradient(45deg, #fff, #a8edea);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .header p {
            font-size: 1.2rem;
            opacity: 0.9;
        }

        .main-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            padding: 2.5rem;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            margin-bottom: 2rem;
        }

        .upload-section {
            text-align: center;
            padding: 3rem 2rem;
            border: 2px dashed rgba(255, 255, 255, 0.3);
            border-radius: 16px;
            transition: all 0.3s ease;
            cursor: pointer;
            margin-bottom: 2rem;
        }

        .upload-section:hover {
            border-color: rgba(255, 255, 255, 0.6);
            background: rgba(255, 255, 255, 0.05);
            transform: translateY(-2px);
        }

        .upload-icon {
            width: 80px;
            height: 80px;
            margin: 0 auto 1rem;
            background: linear-gradient(45deg, #ff6b6b, #feca57);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
        }

        .upload-text {
            font-size: 1.1rem;
            margin-bottom: 0.5rem;
        }

        .upload-subtext {
            opacity: 0.7;
            font-size: 0.9rem;
        }

        .file-input {
            display: none;
        }

        .analyze-btn {
            background: linear-gradient(45deg, #ff6b6b, #feca57);
            border: none;
            padding: 1rem 2rem;
            border-radius: 12px;
            color: white;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 100%;
            margin-top: 1rem;
        }

        .analyze-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(255, 107, 107, 0.3);
        }

        .analyze-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .results {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1.5rem;
            margin-top: 2rem;
        }

        .result-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 1.5rem;
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s ease;
        }

        .result-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
        }

        .result-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .result-icon {
            width: 24px;
            height: 24px;
            background: linear-gradient(45deg, #a8edea, #fed6e3);
            border-radius: 6px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.8rem;
        }

        .progress-bar {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            height: 8px;
            margin: 0.5rem 0;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(45deg, #a8edea, #fed6e3);
            border-radius: 8px;
            transition: width 0.3s ease;
        }

        .genre-tag {
            display: inline-block;
            background: rgba(255, 255, 255, 0.2);
            padding: 0.25rem 0.75rem;
            border-radius: 20px;
            font-size: 0.8rem;
            margin: 0.25rem;
        }

        .loading {
            display: none;
            text-align: center;
            padding: 2rem;
        }

        .loading.active {
            display: block;
        }

        .spinner {
            width: 40px;
            height: 40px;
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-top: 3px solid white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 1rem;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .waveform {
            height: 60px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            margin: 1rem 0;
            display: flex;
            align-items: end;
            padding: 0.5rem;
            gap: 2px;
            overflow: hidden;
        }

        .waveform-bar {
            background: linear-gradient(to top, #ff6b6b, #feca57);
            border-radius: 2px;
            flex: 1;
            animation: wave 2s ease-in-out infinite;
        }

        .waveform-bar:nth-child(odd) {
            animation-delay: 0.1s;
        }

        .waveform-bar:nth-child(even) {
            animation-delay: 0.2s;
        }

        @keyframes wave {
            0%, 100% { height: 20%; }
            50% { height: 80%; }
        }

        @media (max-width: 768px) {
            .container {
                padding: 1rem;
            }
            
            .header h1 {
                font-size: 2rem;
            }
            
            .main-card {
                padding: 1.5rem;
            }
            
            .results {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>AI Music Analyzer</h1>
            <p>Discover the hidden patterns in your music with advanced AI analysis</p>
        </div>

        <div class="main-card">
            <div class="upload-section" onclick="document.getElementById('fileInput').click()">
                <div class="upload-icon">🎵</div>
                <div class="upload-text">Drop your music file here or click to browse</div>
                <div class="upload-subtext">Supports MP3, WAV, FLAC formats</div>
            </div>
            <input type="file" id="fileInput" class="file-input" accept=".mp3,.wav,.flac" onchange="handleFileSelect(event)">
            
            <div id="fileName" style="display: none; text-align: center; margin: 1rem 0; opacity: 0.8;"></div>
            
            <button class="analyze-btn" id="analyzeBtn" onclick="analyzeMusic()" disabled>
                Analyze Music
            </button>

            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p>Analyzing your music... This may take a moment.</p>
            </div>
        </div>

        <div class="results" id="results" style="display: none;">
            <div class="result-card">
                <div class="result-title">
                    <div class="result-icon">🎼</div>
                    Genre Analysis
                </div>
                <div id="genreResults">
                    <span class="genre-tag">Electronic</span>
                    <span class="genre-tag">Ambient</span>
                    <span class="genre-tag">Downtempo</span>
                </div>
            </div>

            <div class="result-card">
                <div class="result-title">
                    <div class="result-icon">⚡</div>
                    Energy & Mood
                </div>
                <div>
                    <div style="margin-bottom: 0.5rem;">Energy: <strong>72%</strong></div>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 72%;"></div>
                    </div>
                    <div style="margin-bottom: 0.5rem;">Valence: <strong>58%</strong></div>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 58%;"></div>
                    </div>
                </div>
            </div>

            <div class="result-card">
                <div class="result-title">
                    <div class="result-icon">🎯</div>
                    Technical Analysis
                </div>
                <div>
                    <div><strong>BPM:</strong> 128</div>
                    <div><strong>Key:</strong> C Major</div>
                    <div><strong>Duration:</strong> 3:24</div>
                    <div><strong>Danceability:</strong> 85%</div>
                </div>
            </div>

            <div class="result-card">
                <div class="result-title">
                    <div class="result-icon">📊</div>
                    Audio Waveform
                </div>
                <div class="waveform">
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                    <div class="waveform-bar"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        let selectedFile = null;

        function handleFileSelect(event) {
            const file = event.target.files[0];
            if (file) {
                selectedFile = file;
                const fileName = document.getElementById('fileName');
                fileName.textContent = `Selected: ${file.name}`;
                fileName.style.display = 'block';
                
                const analyzeBtn = document.getElementById('analyzeBtn');
                analyzeBtn.disabled = false;
                analyzeBtn.textContent = 'Analyze Music';
            }
        }

        function analyzeMusic() {
            if (!selectedFile) return;
            
            const loading = document.getElementById('loading');
            const results = document.getElementById('results');
            const analyzeBtn = document.getElementById('analyzeBtn');
            
            loading.classList.add('active');
            results.style.display = 'none';
            analyzeBtn.disabled = true;
            analyzeBtn.textContent = 'Analyzing...';
            
            // Simulate analysis process
            setTimeout(() => {
                loading.classList.remove('active');
                results.style.display = 'grid';
                analyzeBtn.disabled = false;
                analyzeBtn.textContent = 'Analyze Another Track';
                
                // Animate results appearing
                const cards = document.querySelectorAll('.result-card');
                cards.forEach((card, index) => {
                    card.style.opacity = '0';
                    card.style.transform = 'translateY(20px)';
                    setTimeout(() => {
                        card.style.transition = 'all 0.5s ease';
                        card.style.opacity = '1';
                        card.style.transform = 'translateY(0)';
                    }, index * 100);
                });
            }, 3000);
        }

        // Drag and drop functionality
        const uploadSection = document.querySelector('.upload-section');
        
        uploadSection.addEventListener('dragover', (e) => {
            e.preventDefault();
            uploadSection.style.borderColor = 'rgba(255, 255, 255, 0.8)';
            uploadSection.style.background = 'rgba(255, 255, 255, 0.1)';
        });
        
        uploadSection.addEventListener('dragleave', (e) => {
            e.preventDefault();
            uploadSection.style.borderColor = 'rgba(255, 255, 255, 0.3)';
            uploadSection.style.background = 'transparent';
        });
        
        uploadSection.addEventListener('drop', (e) => {
            e.preventDefault();
            uploadSection.style.borderColor = 'rgba(255, 255, 255, 0.3)';
            uploadSection.style.background = 'transparent';
            
            const files = e.dataTransfer.files;
            if (files.length > 0) {
                const file = files[0];
                if (file.type.includes('audio')) {
                    selectedFile = file;
                    const fileName = document.getElementById('fileName');
                    fileName.textContent = `Selected: ${file.name}`;
                    fileName.style.display = 'block';
                    
                    const analyzeBtn = document.getElementById('analyzeBtn');
                    analyzeBtn.disabled = false;
                    analyzeBtn.textContent = 'Analyze Music';
                }
            }
        });
    </script>
</body>
</html>