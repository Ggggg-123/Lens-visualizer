<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lens Physics Visualizer</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0f172a, #1e293b);
            color: #e2e8f0;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            margin-bottom: 40px;
            padding: 20px;
            background: rgba(30, 41, 59, 0.7);
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 10px;
            background: linear-gradient(90deg, #38bdf8, #818cf8);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        
        .subtitle {
            font-size: 1.2rem;
            color: #94a3b8;
            max-width: 800px;
            margin: 0 auto;
        }
        
        .content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-bottom: 40px;
        }
        
        @media (max-width: 900px) {
            .content {
                grid-template-columns: 1fr;
            }
        }
        
        .visualization-panel {
            background: rgba(30, 41, 59, 0.7);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
        }
        
        .control-panel {
            background: rgba(30, 41, 59, 0.7);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
        }
        
        h2 {
            font-size: 1.8rem;
            margin-bottom: 20px;
            color: #38bdf8;
            border-bottom: 2px solid #334155;
            padding-bottom: 10px;
        }
        
        h3 {
            font-size: 1.4rem;
            margin: 25px 0 15px;
            color: #38bdf8;
        }
        
        p {
            line-height: 1.6;
            margin-bottom: 15px;
        }
        
        .canvas-container {
            position: relative;
            width: 100%;
            height: 400px;
            background: #0f172a;
            border-radius: 10px;
            overflow: hidden;
            margin: 20px 0;
            border: 1px solid #334155;
        }
        
        canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        
        .controls {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 25px;
        }
        
        .control-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #cbd5e1;
        }
        
        input[type="range"] {
            width: 100%;
            height: 8px;
            border-radius: 5px;
            background: #334155;
            outline: none;
            -webkit-appearance: none;
        }
        
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #38bdf8;
            cursor: pointer;
            box-shadow: 0 0 10px rgba(56, 189, 248, 0.5);
        }
        
        .value-display {
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
            font-size: 0.9rem;
            color: #94a3b8;
        }
        
        .formula {
            background: #0f172a;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
            text-align: center;
            border: 1px solid #334155;
        }
        
        .formula-equation {
            font-size: 2.2rem;
            font-weight: bold;
            margin: 15px 0;
            color: #38bdf8;
        }
        
        .variables {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
            flex-wrap: wrap;
        }
        
        .variable {
            text-align: center;
            margin: 10px;
        }
        
        .variable-name {
            font-size: 1.2rem;
            font-weight: bold;
            color: #38bdf8;
        }
        
        .variable-desc {
            font-size: 0.9rem;
            color: #94a3b8;
            margin-top: 5px;
        }
        
        .camera-breakdown {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 30px;
        }
        
        .camera-part {
            background: #0f172a;
            padding: 15px;
            border-radius: 10px;
            border-left: 4px solid #38bdf8;
        }
        
        .part-title {
            font-weight: bold;
            margin-bottom: 8px;
            color: #38bdf8;
        }
        
        .conclusion {
            background: rgba(30, 41, 59, 0.7);
            border-radius: 15px;
            padding: 25px;
            margin-top: 40px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
        }
        
        .key-takeaways {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 25px;
        }
        
        .takeaway {
            background: #0f172a;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            border-top: 4px solid #38bdf8;
        }
        
        .takeaway-icon {
            font-size: 2.5rem;
            margin-bottom: 15px;
        }
        
        .tab-container {
            margin-top: 20px;
        }
        
        .tabs {
            display: flex;
            border-bottom: 1px solid #334155;
            margin-bottom: 20px;
        }
        
        .tab {
            padding: 12px 20px;
            cursor: pointer;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
        }
        
        .tab.active {
            border-bottom: 3px solid #38bdf8;
            color: #38bdf8;
            font-weight: bold;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        button {
            background: #38bdf8;
            color: #0f172a;
            border: none;
            padding: 12px 20px;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 10px;
            width: 100%;
        }
        
        button:hover {
            background: #0ea5e9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(56, 189, 248, 0.4);
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Lens Physics Visualizer</h1>
            <p class="subtitle">Explore how a simple piece of plastic can become a camera lens through the magic of optics and the lens formula</p>
        </header>
        
        <div class="content">
            <div class="visualization-panel">
                <h2>Lens Formula Visualization</h2>
                <p>Adjust the parameters below to see how the lens formula works in real-time. Watch how changing the object distance affects where the image forms.</p>
                
                <div class="canvas-container">
                    <canvas id="lensCanvas"></canvas>
                </div>
                
                <div class="formula">
                    <p>The Universal Lens Formula</p>
                    <div class="formula-equation">1/f = 1/v + 1/u</div>
                    <p>This equation defines the relationship between the object, the lens, and the image</p>
                </div>
                
                <div class="variables">
                    <div class="variable">
                        <div class="variable-name">f</div>
                        <div class="variable-desc">Focal Length</div>
                    </div>
                    <div class="variable">
                        <div class="variable-name">v</div>
                        <div class="variable-desc">Image Distance</div>
                    </div>
                    <div class="variable">
                        <div class="variable-name">u</div>
                        <div class="variable-desc">Object Distance</div>
                    </div>
                </div>
            </div>
            
            <div class="control-panel">
                <h2>Interactive Controls</h2>
                
                <div class="controls">
                    <div class="control-group">
                        <label for="focalLength">Focal Length (f)</label>
                        <input type="range" id="focalLength" min="100" max="300" value="200">
                        <div class="value-display">
                            <span>Short</span>
                            <span id="focalLengthValue">200 mm</span>
                            <span>Long</span>
                        </div>
                    </div>
                    
                    <div class="control-group">
                        <label for="objectDistance">Object Distance (u)</label>
                        <input type="range" id="objectDistance" min="300" max="800" value="500">
                        <div class="value-display">
                            <span>Close</span>
                            <span id="objectDistanceValue">500 mm</span>
                            <span>Far</span>
                        </div>
                    </div>
                </div>
                
                <div class="tab-container">
                    <div class="tabs">
                        <div class="tab active" data-tab="physics">Physics Explained</div>
                        <div class="tab" data-tab="camera">Simple Camera Design</div>
                    </div>
                    
                    <div class="tab-content active" id="physics-content">
                        <h3>How Lenses Bend Light</h3>
                        <p>Lenses work through <strong>refraction</strong> - the bending of light as it passes from one medium to another (like air to plastic).</p>
                        
                        <p>A <strong>convex lens</strong> (curved outward) converges light rays toward a single point called the <strong>focal point</strong>.</p>
                        
                        <p>The distance from the center of the lens to this focal point is the <strong>focal length (f)</strong>, which defines the "personality" of the lens.</p>
                        
                        <h3>The Lens Formula in Action</h3>
                        <p>When you change the object distance (u), the image distance (v) must adjust according to the formula to maintain a sharp image.</p>
                        
                        <p>This is the fundamental physics behind "focusing" a camera!</p>
                    </div>
                    
                    <div class="tab-content" id="camera-content">
                        <h3>From Physics to Product</h3>
                        <p>Professional camera lenses use multiple elements to correct distortions, but simple cameras use a clever design with just one plastic lens.</p>
                        
                        <div class="camera-breakdown">
                            <div class="camera-part">
                                <div class="part-title">1. Single Element Aspherical Lens</div>
                                <p>Molded plastic, cheap to produce. "Aspherical" shape reduces distortions compared to a simple curve.</p>
                            </div>
                            <div class="camera-part">
                                <div class="part-title">2. Tiny Aperture</div>
                                <p>A small hole that blocks blurry light rays from lens edges, allowing only clean central rays through.</p>
                            </div>
                            <div class="camera-part">
                                <div class="part-title">3. Fixed Sensor</div>
                                <p>Placed at the focal length of the lens. With the tiny aperture's depth of field, it works for most scenarios.</p>
                            </div>
                            <div class="camera-part">
                                <div class="part-title">4. Software Correction</div>
                                <p>Any remaining distortions are cleaned up by the phone's Image Signal Processor (ISP).</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <button id="resetBtn">Reset to Default Values</button>
            </div>
        </div>
        
        <div class="conclusion">
            <h2>Key Takeaways</h2>
            <p>The simple camera lens demonstrates that brilliant design isn't about adding complexity, but about using physics smartly to create affordable, functional technology.</p>
            
            <div class="key-takeaways">
                <div class="takeaway">
                    <div class="takeaway-icon">⚡</div>
                    <div class="takeaway-title">Universal Physics</div>
                    <p>The Lens Formula governs all image formation, from simple to complex lenses.</p>
                </div>
                <div class="takeaway">
                    <div class="takeaway-icon">🔍</div>
                    <div class="takeaway-title">Smart Simplification</div>
                    <p>A single plastic element with a tiny aperture can replace complex multi-element designs.</p>
                </div>
                <div class="takeaway">
                    <div class="takeaway-icon">💡</div>
                    <div class="takeaway-title">Ingenious Engineering</div>
                    <p>Limitations are overcome through clever design choices, not just more components.</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Get DOM elements
        const canvas = document.getElementById('lensCanvas');
        const ctx = canvas.getContext('2d');
        const focalLengthSlider = document.getElementById('focalLength');
        const objectDistanceSlider = document.getElementById('objectDistance');
        const focalLengthValue = document.getElementById('focalLengthValue');
        const objectDistanceValue = document.getElementById('objectDistanceValue');
        const resetBtn = document.getElementById('resetBtn');
        const tabs = document.querySelectorAll('.tab');
        const tabContents = document.querySelectorAll('.tab-content');
        
        // Initialize values
        let focalLength = parseInt(focalLengthSlider.value);
        let objectDistance = parseInt(objectDistanceSlider.value);
        let imageDistance = calculateImageDistance(focalLength, objectDistance);
        
        // Set canvas dimensions
        function setCanvasDimensions() {
            const container = canvas.parentElement;
            canvas.width = container.clientWidth;
            canvas.height = container.clientHeight;
            drawLensDiagram();
        }
        
        // Calculate image distance using lens formula
        function calculateImageDistance(f, u) {
            return 1 / (1/f - 1/u);
        }
        
        // Draw the lens diagram
        function drawLensDiagram() {
            const width = canvas.width;
            const height = canvas.height;
            
            // Clear canvas
            ctx.clearRect(0, 0, width, height);
            
            // Calculate positions
            const lensCenterX = width * 0.3;
            const objectX = width * 0.1;
            const imageX = lensCenterX + imageDistance / 5; // Scale for visualization
            
            // Draw optical axis
            ctx.beginPath();
            ctx.moveTo(0, height/2);
            ctx.lineTo(width, height/2);
            ctx.strokeStyle = '#475569';
            ctx.lineWidth = 1;
            ctx.stroke();
            
            // Draw lens
            ctx.beginPath();
            ctx.moveTo(lensCenterX, height/2 - 100);
            ctx.lineTo(lensCenterX, height/2 + 100);
            ctx.strokeStyle = '#38bdf8';
            ctx.lineWidth = 4;
            ctx.stroke();
            
            // Draw focal points
            const focalPoint1X = lensCenterX - focalLength / 5;
            const focalPoint2X = lensCenterX + focalLength / 5;
            
            ctx.beginPath();
            ctx.arc(focalPoint1X, height/2, 5, 0, Math.PI * 2);
            ctx.fillStyle = '#f59e0b';
            ctx.fill();
            
            ctx.beginPath();
            ctx.arc(focalPoint2X, height/2, 5, 0, Math.PI * 2);
            ctx.fillStyle = '#f59e0b';
            ctx.fill();
            
            // Draw object (arrow)
            const objectHeight = 60;
            ctx.beginPath();
            ctx.moveTo(objectX, height/2);
            ctx.lineTo(objectX, height/2 - objectHeight);
            ctx.lineTo(objectX + 10, height/2 - objectHeight + 15);
            ctx.moveTo(objectX, height/2 - objectHeight);
            ctx.lineTo(objectX - 10, height/2 - objectHeight + 15);
            ctx.strokeStyle = '#10b981';
            ctx.lineWidth = 2;
            ctx.stroke();
            
            // Draw image (arrow)
            const imageHeight = -objectHeight * (imageDistance / objectDistance);
            ctx.beginPath();
            ctx.moveTo(imageX, height/2);
            ctx.lineTo(imageX, height/2 + imageHeight);
            ctx.lineTo(imageX + 10, height/2 + imageHeight - 15);
            ctx.moveTo(imageX, height/2 + imageHeight);
            ctx.lineTo(imageX - 10, height/2 + imageHeight - 15);
            ctx.strokeStyle = '#ef4444';
            ctx.lineWidth = 2;
            ctx.stroke();
            
            // Draw light rays
            // Ray 1: Parallel to axis, through focal point
            ctx.beginPath();
            ctx.moveTo(objectX, height/2 - objectHeight);
            ctx.lineTo(lensCenterX, height/2 - objectHeight);
            ctx.lineTo(imageX, height/2 + imageHeight);
            ctx.strokeStyle = 'rgba(248, 113, 113, 0.7)';
            ctx.lineWidth = 1.5;
            ctx.stroke();
            
            // Ray 2: Through center of lens (no refraction)
            ctx.beginPath();
            ctx.moveTo(objectX, height/2 - objectHeight);
            ctx.lineTo(lensCenterX, height/2);
            ctx.lineTo(imageX, height/2 + imageHeight);
            ctx.strokeStyle = 'rgba(34, 197, 94, 0.7)';
            ctx.lineWidth = 1.5;
            ctx.stroke();
            
            // Ray 3: Through focal point, then parallel
            ctx.beginPath();
            ctx.moveTo(objectX, height/2 - objectHeight);
            ctx.lineTo(focalPoint1X, height/2);
            ctx.lineTo(lensCenterX, height/2 + (height/2 - objectHeight) / 4);
            ctx.lineTo(imageX, height/2 + imageHeight);
            ctx.strokeStyle = 'rgba(56, 189, 248, 0.7)';
            ctx.lineW
