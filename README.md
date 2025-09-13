<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>시간-공간 거울 실험</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            background: radial-gradient(circle at center, #000000, #001122, #002244, #000000);
            color: #fff;
            font-family: 'Times New Roman', serif;
            overflow: hidden;
            animation: cosmicBreathing 6s ease-in-out infinite;
        }
        
        @keyframes cosmicBreathing {
            0%, 100% { filter: hue-rotate(0deg) brightness(1); }
            33% { filter: hue-rotate(120deg) brightness(1.2); }
            66% { filter: hue-rotate(240deg) brightness(0.8); }
        }
        
        #container {
            position: relative;
            width: 100vw;
            height: 100vh;
        }
        
        #temporal-control {
            position: absolute;
            top: 20px;
            left: 20px;
            z-index: 100;
            background: rgba(0,0,50,0.95);
            border: 3px solid #ffffff;
            border-radius: 20px;
            padding: 25px;
            max-width: 400px;
            box-shadow: 0 0 40px rgba(255,255,255,0.3);
            backdrop-filter: blur(20px);
        }
        
        #space-expansion {
            position: absolute;
            top: 20px;
            right: 20px;
            z-index: 100;
            background: rgba(50,0,50,0.95);
            border: 3px solid #ff00ff;
            border-radius: 20px;
            padding: 25px;
            max-width: 450px;
            box-shadow: 0 0 40px rgba(255,0,255,0.3);
            backdrop-filter: blur(20px);
        }
        
        #destiny-analysis {
            position: absolute;
            bottom: 20px;
            left: 20px;
            z-index: 100;
            background: rgba(50,50,0,0.95);
            border: 3px solid #ffff00;
            border-radius: 20px;
            padding: 25px;
            max-width: 500px;
            max-height: 350px;
            overflow-y: auto;
            box-shadow: 0 0 40px rgba(255,255,0,0.3);
            backdrop-filter: blur(20px);
        }
        
        #consciousness-mirror {
            position: absolute;
            bottom: 20px;
            right: 20px;
            z-index: 100;
            background: rgba(0,50,50,0.95);
            border: 3px solid #00ffff;
            border-radius: 20px;
            padding: 25px;
            max-width: 450px;
            box-shadow: 0 0 40px rgba(0,255,255,0.3);
            backdrop-filter: blur(20px);
        }
        
        .section-title {
            font-size: 20px;
            font-weight: bold;
            margin-bottom: 20px;
            text-shadow: 0 0 20px currentColor;
            animation: titlePulse 3s ease-in-out infinite alternate;
            text-align: center;
        }
        
        @keyframes titlePulse {
            0% { opacity: 0.8; transform: scale(1); }
            100% { opacity: 1; transform: scale(1.05); text-shadow: 0 0 30px currentColor; }
        }
        
        .time-slider {
            width: 100%;
            margin: 15px 0;
            appearance: none;
            height: 8px;
            border-radius: 5px;
            background: linear-gradient(90deg, #ff0000, #ffff00, #00ff00, #00ffff, #0000ff);
            outline: none;
        }
        
        .time-slider::-webkit-slider-thumb {
            appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #ffffff;
            cursor: pointer;
            box-shadow: 0 0 15px rgba(255,255,255,0.8);
        }
        
        .temporal-display {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 15px;
            margin: 20px 0;
        }
        
        .time-window {
            background: rgba(0,0,0,0.7);
            border: 2px solid;
            border-radius: 15px;
            padding: 15px;
            text-align: center;
            min-height: 120px;
            animation: windowGlow 4s ease-in-out infinite;
        }
        
        @keyframes windowGlow {
            0%, 100% { box-shadow: 0 0 10px currentColor; }
            50% { box-shadow: 0 0 25px currentColor, inset 0 0 15px rgba(255,255,255,0.1); }
        }
        
        .past-window {
            border-color: #ff6666;
            color: #ff6666;
        }
        
        .present-window {
            border-color: #66ff66;
            color: #66ff66;
        }
        
        .future-window {
            border-color: #6666ff;
            color: #6666ff;
        }
        
        .space-dimension {
            display: flex;
            align-items: center;
            margin: 12px 0;
            font-size: 14px;
        }
        
        .dimension-label {
            width: 80px;
            font-weight: bold;
        }
        
        .dimension-value {
            flex: 1;
            text-align: right;
            font-family: 'Courier New', monospace;
        }
        
        .infinity-indicator {
            font-size: 32px;
            text-align: center;
            margin: 20px 0;
            animation: infinityRotate 8s linear infinite;
        }
        
        @keyframes infinityRotate {
            0% { transform: rotate(0deg) scale(1); }
            50% { transform: rotate(180deg) scale(1.2); }
            100% { transform: rotate(360deg) scale(1); }
        }
        
        .destiny-meter {
            display: flex;
            align-items: center;
            margin: 15px 0;
            padding: 10px;
            background: rgba(255,255,255,0.05);
            border-radius: 10px;
        }
        
        .destiny-label {
            width: 120px;
            font-size: 14px;
        }
        
        .destiny-bar {
            flex: 1;
            height: 8px;
            background: rgba(255,255,255,0.2);
            border-radius: 4px;
            margin: 0 10px;
            overflow: hidden;
        }
        
        .destiny-fill {
            height: 100%;
            transition: width 1s ease-in-out;
            animation: destinyPulse 2s ease-in-out infinite alternate;
        }
        
        @keyframes destinyPulse {
            0% { opacity: 0.7; }
            100% { opacity: 1; filter: brightness(1.3); }
        }
        
        .consciousness-layer {
            margin: 15px 0;
            padding: 12px;
            border: 1px solid rgba(255,255,255,0.3);
            border-radius: 10px;
            background: rgba(255,255,255,0.05);
        }
        
        .observer-state {
            display: flex;
            justify-content: space-between;
            margin: 8px 0;
            font-size: 13px;
        }
        
        .control-matrix {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin: 20px 0;
        }
        
        .control-btn {
            background: linear-gradient(45deg, rgba(255,255,255,0.1), rgba(255,255,255,0.3));
            border: 1px solid rgba(255,255,255,0.5);
            color: white;
            padding: 12px;
            border-radius: 15px;
            cursor: pointer;
            font-family: inherit;
            font-size: 12px;
            transition: all 0.3s;
            backdrop-filter: blur(10px);
        }
        
        .control-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255,255,255,0.2);
            background: linear-gradient(45deg, rgba(255,255,255,0.2), rgba(255,255,255,0.4));
        }
        
        .control-btn.active {
            background: linear-gradient(45deg, #00ff00, #00aa00);
            color: #000;
            font-weight: bold;
            box-shadow: 0 0 20px #00ff00;
        }
        
        .philosophical-insight {
            font-style: italic;
            color: #ffdd88;
            margin: 12px 0;
            padding: 10px;
            border-left: 4px solid #ffdd88;
            background: rgba(255,221,136,0.1);
            animation: wisdomGlow 5s ease-in-out infinite alternate;
        }
        
        @keyframes wisdomGlow {
            0% { opacity: 0.8; }
            100% { opacity: 1; box-shadow: 0 0 15px rgba(255,221,136,0.3); }
        }
        
        #temporal-equation {
            font-family: 'Times New Roman', serif;
            font-size: 18px;
            text-align: center;
            margin: 20px 0;
            color: #00ffff;
            animation: equationGlow 4s ease-in-out infinite;
        }
        
        @keyframes equationGlow {
            0%, 100% { text-shadow: 0 0 15px #00ffff; }
            50% { text-shadow: 0 0 30px #00ffff, 0 0 45px #00ffff; }
        }
    </style>
</head>
<body>
    <div id="container">
        <!-- 시간 제어 패널 -->
        <div id="temporal-control">
            <div class="section-title" style="color: #ffffff;">⏰ 시간 변수 제어</div>
            
            <div>시간 흐름 속도:</div>
            <input type="range" class="time-slider" id="time-speed" min="-200" max="200" value="0" 
                   oninput="controlTemporalFlow(this.value)">
            <div style="display: flex; justify-content: space-between; font-size: 11px; margin-top: 5px;">
                <span>← 과거로</span><span>현재</span><span>미래로 →</span>
            </div>
            
            <div>공간 확장 계수:</div>
            <input type="range" class="time-slider" id="space-expansion" min="1" max="1000" value="1" 
                   oninput="expandMirrorSpace(this.value)">
            
            <div id="temporal-equation">
                t' = γ(t - vx/c²)
            </div>
            <div style="text-align: center; font-size: 12px; color: #cccccc;">
                로렌츠 시간 변환
            </div>
            
            <div class="control-matrix">
                <button class="control-btn" onclick="observePast()">과거 관찰</button>
                <button class="control-btn" onclick="observeFuture()">미래 관찰</button>
                <button class="control-btn" onclick="synchronizeTime()">시간 동기화</button>
                <button class="control-btn active" onclick="activateInfinity()">무한대 활성화</button>
            </div>
        </div>
        
        <!-- 공간 확장 패널 -->
        <div id="space-expansion">
            <div class="section-title" style="color: #ff00ff;">🌌 거울 공간 확장</div>
            
            <div class="temporal-display">
                <div class="time-window past-window">
                    <div><strong>과거</strong></div>
                    <div id="past-events">관찰 중...</div>
                    <div id="past-distance">∞ km</div>
                </div>
                <div class="time-window present-window">
                    <div><strong>현재</strong></div>
                    <div id="present-state">실시간</div>
                    <div id="present-position">기준점</div>
                </div>
                <div class="time-window future-window">
                    <div><strong>미래</strong></div>
                    <div id="future-possibilities">예측 중...</div>
                    <div id="future-distance">∞ km</div>
                </div>
            </div>
            
            <div style="margin: 20px 0;">
                <div class="space-dimension">
                    <div class="dimension-label">X축:</div>
                    <div class="dimension-value" id="x-dimension">∞</div>
                </div>
                <div class="space-dimension">
                    <div class="dimension-label">Y축:</div>
                    <div class="dimension-value" id="y-dimension">∞</div>
                </div>
                <div class="space-dimension">
                    <div class="dimension-label">Z축:</div>
                    <div class="dimension-value" id="z-dimension">∞</div>
                </div>
                <div class="space-dimension">
                    <div class="dimension-label">시간축:</div>
                    <div class="dimension-value" id="t-dimension">∞</div>
                </div>
            </div>
            
            <div class="infinity-indicator">∞⁴</div>
        </div>
        
        <!-- 운명 분석 패널 -->
        <div id="destiny-analysis">
            <div class="section-title" style="color: #ffff00;">📜 지천명 vs 자유의지</div>
            
            <div class="destiny-meter">
                <div class="destiny-label">하늘의 뜻:</div>
                <div class="destiny-bar">
                    <div class="destiny-fill" id="destiny-fill" style="width: 50%; background: linear-gradient(90deg, #ff6666, #ffff66);"></div>
                </div>
                <div id="destiny-percent">50%</div>
            </div>
            
            <div class="destiny-meter">
                <div class="destiny-label">자유의지:</div>
                <div class="destiny-bar">
                    <div class="destiny-fill" id="freewill-fill" style="width: 50%; background: linear-gradient(90deg, #66ff66, #66ffff);"></div>
                </div>
                <div id="freewill-percent">50%</div>
            </div>
            
            <div class="philosophical-insight">
                "과거를 보면 운명이 보이고, 미래를 보면 가능성이 보인다"
            </div>
            
            <div class="philosophical-insight">
                "거울 속 무한대는 모든 선택의 결과를 동시에 보여준다"
            </div>
            
            <div style="margin: 20px 0;">
                <div><strong>관찰된 패턴:</strong></div>
                <div id="pattern-analysis">
                    <div>• 시간이 느려질수록 운명의 비중 증가</div>
                    <div>• 공간이 확장될수록 선택의 여지 증가</div>
                    <div>• 무한대에서는 모든 것이 동시에 가능</div>
                </div>
            </div>
            
            <div style="text-align: center; margin: 15px 0;">
                <div style="font-size: 16px; color: #ffcc00;">
                    현재 움직임의 의미: <span id="movement-meaning">탐구 중...</span>
                </div>
            </div>
        </div>
        
        <!-- 의식 거울 패널 -->
        <div id="consciousness-mirror">
            <div class="section-title" style="color: #00ffff;">🪞 의식의 이중 관찰</div>
            
            <div class="consciousness-layer">
                <div><strong>관찰하는 나</strong></div>
                <div class="observer-state">
                    <span>위치:</span><span id="observer-position">고정점</span>
                </div>
                <div class="observer-state">
                    <span>시점:</span><span id="observer-perspective">객관적</span>
                </div>
                <div class="observer-state">
                    <span>인식:</span><span id="observer-awareness">제한적</span>
                </div>
            </div>
            
            <div class="consciousness-layer">
                <div><strong>거울 속 나</strong></div>
                <div class="observer-state">
                    <span>위치:</span><span id="mirror-position">무한 공간</span>
                </div>
                <div class="observer-state">
                    <span>시점:</span><span id="mirror-perspective">전지적</span>
                </div>
                <div class="observer-state">
                    <span>인식:</span><span id="mirror-awareness">무한대</span>
                </div>
            </div>
            
            <div style="margin: 20px 0;">
                <div><strong>느끼는 차이:</strong></div>
                <div id="consciousness-difference">
                    <div>• 시간의 흐름이 다르게 느껴짐</div>
                    <div>• 공간의 크기가 상대적으로 변함</div>
                    <div>• 과거와 미래가 동시에 보임</div>
                    <div>• 모든 가능성이 중첩되어 나타남</div>
                </div>
            </div>
            
            <div style="text-align: center; margin: 15px 0; padding: 15px; background: rgba(255,255,255,0.1); border-radius: 10px;">
                <div style="font-size: 18px; color: #ffffff;">🔮</div>
                <div style="font-size: 14px;">
                    두 의식이 하나가 되는 순간<br>
                    진실이 드러납니다
                </div>
            </div>
        </div>
    </div>

    <script>
        let scene, camera, renderer;
        let mirrorSpheres = [];
        let temporalParticles = [];
        let spaceExpansion = 1;
        let timeFlow = 0;
        let infinityActivated = false;
        let observationData = {
            destiny: 50,
            freewill: 50,
            pastEvents: [],
            futureEvents: [],
            consciousness: 'unified'
        };
        
        function init() {
            // Three.js 초기화
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 10000);
            renderer = new THREE.WebGLRenderer({antialias: true, alpha: true});
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setClearColor(0x000000, 0);
            document.getElementById('container').appendChild(renderer.domElement);
            
            createInfiniteMirrorSpace();
            createTemporalParticles();
            
            camera.position.set(0, 0, 15);
            animate();
            
            // 실시간 분석
            setInterval(analyzeTemporalState, 500);
            setInterval(updateConsciousnessObservation, 1000);
        }
        
        // 무한 거울 공간 생성
        function createInfiniteMirrorSpace() {
            // 중심 거울 구
            const centralGeometry = new THREE.SphereGeometry(2, 32, 32);
            const centralMaterial = new THREE.MeshPhongMaterial({
                color: 0xffffff,
                transparent: true,
                opacity: 0.1,
                reflectivity: 1.0
            });
            const centralMirror = new THREE.Mesh(centralGeometry, centralMaterial);
            scene.add(centralMirror);
            
            // 무한 확장 거울들
            for(let layer = 1; layer <= 10; layer++) {
                for(let i = 0; i < layer * 8; i++) {
                    const geometry = new THREE.SphereGeometry(0.5, 16, 16);
                    const material = new THREE.MeshPhongMaterial({
                        color: new THREE.Color().setHSL((layer * 0.1 + i * 0.05) % 1, 0.8, 0.6),
                        transparent: true,
                        opacity: 0.3 - layer * 0.02,
                        reflectivity: 0.9
                    });
                    
                    const mirror = new THREE.Mesh(geometry, material);
                    const angle = (i / (layer * 8)) * Math.PI * 2;
                    const radius = layer * 3;
                    
                    mirror.position.set(
                        Math.cos(angle) * radius,
                        Math.sin(angle) * radius,
                        (Math.random() - 0.5) * layer * 2
                    );
                    
                    mirror.userData = {
                        layer: layer,
                        originalRadius: radius,
                        angle: angle,
                        timePhase: Math.random() * Math.PI * 2
                    };
                    
                    mirrorSpheres.push(mirror);
                    scene.add(mirror);
                }
            }
            
            // 조명 (시공간 조명)
            const ambientLight = new THREE.AmbientLight(0x404040, 0.4);
            scene.add(ambientLight);
            
            for(let i = 0; i < 6; i++) {
                const light = new THREE.PointLight(
                    new THREE.Color().setHSL(i * 0.16, 1, 0.5),
                    1,
                    100
                );
                light.position.set(
                    Math.cos(i * Math.PI / 3) * 20,
                    Math.sin(i * Math.PI / 3) * 20,
                    (i % 2) * 10 - 5
                );
                scene.add(light);
            }
        }
        
        // 시간 입자들 생성
        function createTemporalParticles() {
            for(let i = 0; i < 200; i++) {
                const geometry = new THREE.SphereGeometry(0.02, 8, 8);
                const material = new THREE.MeshBasicMaterial({
                    color: new THREE.Color().setHSL(Math.random(), 1, 0.8),
                    transparent: true,
                    opacity: 0.7
                });
                
                const particle = new THREE.Mesh(geometry, material);
                particle.position.set(
                    (Math.random() - 0.5) * 100,
                    (Math.random() - 0.5) * 100,
                    (Math.random() - 0.5) * 100
                );
                
                particle.userData = {
                    velocity: new THREE.Vector3(
                        (Math.random() - 0.5) * 0.1,
                        (Math.random() - 0.5) * 0.1,
                        (Math.random() - 0.5) * 0.1
                    ),
                    timeOrigin: Math.random() * 1000,
                    temporalPhase: Math.random() * Math.PI * 2
                };
                
                temporalParticles.push(particle);
                scene.add(particle);
            }
        }
        
        // 시간 흐름 제어
        function controlTemporalFlow(value) {
            timeFlow = parseFloat(value) / 100;
            
            // 운명과 자유의지 비율 계산
            const destinyRatio = 50 + Math.abs(timeFlow) * 30;
            const freewillRatio = 100 - destinyRatio;
            
            observationData.destiny = Math.min(100, destinyRatio);
            observationData.freewill = Math.max(0, freewillRatio);
            
            updateDestinyDisplay();
            updateTemporalWindows();
        }
        
        // 거울 공간 확장
        function expandMirrorSpace(value) {
            spaceExpansion = parseFloat(value);
            
            // 차원 값 업데이트
            const dimensionValue = spaceExpansion === 1000 ? '∞' : `${spaceExpansion.toFixed(1)}×10¹²`;
            document.getElementById('x-dimension').textContent = dimensionValue;
            document.getElementById('y-dimension').textContent = dimensionValue;
            document.getElementById('z-dimension').textContent = dimensionValue;
            document.getElementById('t-dimension').textContent = spaceExpansion === 1000 ? '∞' : `${(spaceExpansion * timeFlow).toFixed(1)}×10¹²`;
            
            // 거울들 위치 조정
            mirrorSpheres.forEach(mirror => {
                const userData = mirror.userData;
                const newRadius = userData.originalRadius * (1 + spaceExpansion * 0.01);
                mirror.position.set(
                    Math.cos(userData.angle) * newRadius,
                    Math.sin(userData.angle) * newRadius,
                    mirror.position.z * (1 + spaceExpansion * 0.001)
                );
            });
        }
        
        // 과거 관찰
        function observePast() {
            const pastEvents = [
                '첫 번째 선택의 순간',
                '운명의 갈래점',
                '의식의 각성',
                '시공간의 굴절',
                '거울의 첫 반사'
            ];
            
            const randomEvent = pastEvents[Math.floor(Math.random() * pastEvents.length)];
            document.getElementById('past-events').textContent = randomEvent;
            document.getElementById('past-distance').textContent = `${(Math.random() * 1000000).toFixed(0)} 광년`;
            
            // 운명 비중 증가
            observationData.destiny = Math.min(100, observationData.destiny + 10);
            updateDestinyDisplay();
        }
        
        // 미래 관찰
        function observeFuture() {
            const futureEvents = [
                '무한 가능성의 수렴',
                '의식의 통합',
                '시간의 해방',
                '공간의 초월',
                '완전한 깨달음'
            ];
            
            const randomEvent = futureEvents[Math.floor(Math.random() * futureEvents.length)];
            document.getElementById('future-possibilities').textContent = randomEvent;
            document.getElementById('future-distance').textContent = `${(Math.random() * 1000000).toFixed(0)} 광년`;
            
            // 자유의지 비중 증가
            observationData.freewill = Math.min(100, observationData.freewill + 10);
            updateDestinyDisplay();
        }
        
        // 시간 동기화
        function synchronizeTime() {
            timeFlow = 0;
            document.getElementById('time-speed').value = 0;
            
            // 균형 상태로 복귀
            observationData.destiny = 50;
            observationData.freewill = 50;
            updateDestinyDisplay();
            
            alert('모든 시간대가 동기화되었습니다.\n과거-현재-미래가 하나의 순간으로 수렴합니다.');
        }
        
        // 무한대 활성화
        function activateInfinity() {
            infinityActivated = !infinityActivated;
            
            if (infinityActivated) {
                spaceExpansion = 1000;
                document.getElementById('space-expansion').value = 1000;
                expandMirrorSpace(1000);
                
                // 모든 거울들을 무한대로 확장
                mirrorSpheres.forEach(mirror => {
                    mirror.scale.setScalar(1 + Math.random() * 5);
                    mirror.material.opacity = 0.1 + Math.random() * 0.3;
                });
                
                alert('무한대가 활성화되었습니다!\n모든 가능성이 동시에 관찰됩니다.');
            }
        }
        
        // 운명 표시 업데이트
        function updateDestinyDisplay() {