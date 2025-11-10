<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>4024俱乐部 - 高级抽奖系统</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0c0c0c 0%, #1a1a2e 50%, #16213e 100%);
            color: #e0e0e0;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            overflow-x: hidden;
        }
        
        .container {
            max-width: 800px;
            width: 100%;
            text-align: center;
        }
        
        .header {
            margin-bottom: 40px;
            position: relative;
        }
        
        .club-name {
            font-size: 3.5rem;
            font-weight: 300;
            letter-spacing: 8px;
            margin-bottom: 10px;
            color: #f8f8f8;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.3);
            position: relative;
            display: inline-block;
        }
        
        .club-name::after {
            content: '';
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 100px;
            height: 2px;
            background: linear-gradient(90deg, transparent, #d4af37, transparent);
        }
        
        .subtitle {
            font-size: 1.2rem;
            letter-spacing: 4px;
            color: #d4af37;
            margin-top: 15px;
            font-weight: 300;
        }
        
        .lottery-machine {
            background: rgba(30, 30, 46, 0.7);
            border-radius: 20px;
            padding: 40px 30px;
            margin: 30px 0;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            border: 1px solid rgba(212, 175, 55, 0.2);
            position: relative;
            overflow: hidden;
        }
        
        .lottery-machine::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, #d4af37, #f8f8f8, #d4af37, #f8f8f8);
            z-index: -1;
            border-radius: 22px;
            opacity: 0.1;
        }
        
        .number-display {
            font-size: 8rem;
            font-weight: 700;
            color: #d4af37;
            text-shadow: 0 0 20px rgba(212, 175, 55, 0.5);
            height: 180px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 20px 0;
            background: rgba(20, 20, 35, 0.6);
            border-radius: 15px;
            border: 1px solid rgba(212, 175, 55, 0.3);
            transition: all 0.5s ease;
        }
        
        .draw-button {
            background: linear-gradient(135deg, #d4af37 0%, #f9e076 100%);
            color: #1a1a2e;
            border: none;
            padding: 18px 50px;
            font-size: 1.3rem;
            font-weight: 600;
            border-radius: 50px;
            cursor: pointer;
            margin: 30px 0;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(212, 175, 55, 0.4);
            letter-spacing: 2px;
            position: relative;
            overflow: hidden;
        }
        
        .draw-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(212, 175, 55, 0.6);
        }
        
        .draw-button:active {
            transform: translateY(1px);
        }
        
        .draw-button::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
            transition: 0.5s;
        }
        
        .draw-button:hover::after {
            left: 100%;
        }
        
        .history {
            margin-top: 40px;
            text-align: left;
        }
        
        .history h3 {
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: #d4af37;
            text-align: center;
            font-weight: 400;
            letter-spacing: 2px;
        }
        
        .history-list {
            max-height: 200px;
            overflow-y: auto;
            padding: 15px;
            background: rgba(20, 20, 35, 0.6);
            border-radius: 10px;
            border: 1px solid rgba(212, 175, 55, 0.2);
        }
        
        .history-item {
            padding: 10px 15px;
            border-bottom: 1px solid rgba(212, 175, 55, 0.1);
            display: flex;
            justify-content: space-between;
        }
        
        .history-item:last-child {
            border-bottom: none;
        }
        
        .history-number {
            color: #d4af37;
            font-weight: 600;
        }
        
        .history-time {
            color: #a0a0a0;
            font-size: 0.9rem;
        }
        
        .footer {
            margin-top: 40px;
            color: #666;
            font-size: 0.9rem;
            text-align: center;
        }
        
        .particles {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
        }
        
        .particle {
            position: absolute;
            background: rgba(212, 175, 55, 0.3);
            border-radius: 50%;
            animation: float 15s infinite linear;
        }
        
        @keyframes float {
            0% {
                transform: translateY(0) rotate(0deg);
                opacity: 0;
            }
            10% {
                opacity: 1;
            }
            90% {
                opacity: 1;
            }
            100% {
                transform: translateY(-1000px) rotate(720deg);
                opacity: 0;
            }
        }
        
        /* 响应式设计 */
        @media (max-width: 768px) {
            .club-name {
                font-size: 2.5rem;
                letter-spacing: 5px;
            }
            
            .number-display {
                font-size: 6rem;
                height: 150px;
            }
            
            .draw-button {
                padding: 15px 40px;
                font-size: 1.1rem;
            }
        }
        
        @media (max-width: 480px) {
            .club-name {
                font-size: 2rem;
                letter-spacing: 3px;
            }
            
            .number-display {
                font-size: 5rem;
                height: 120px;
            }
            
            .lottery-machine {
                padding: 30px 20px;
            }
        }
    </style>
</head>
<body>
    <div class="particles" id="particles"></div>
    
    <div class="container">
        <div class="header">
            <h1 class="club-name">4024 CLUB</h1>
            <p class="subtitle">ADZM SPIRITS CLUB</p>
        </div>
        
        <div class="lottery-machine">
            <div class="number-display" id="numberDisplay">?</div>
            <button class="draw-button" id="drawButton">开始抽奖</button>
        </div>
        
        <div class="history">
            <h3>抽奖记录</h3>
            <div class="history-list" id="historyList">
                <!-- 历史记录将通过JavaScript动态添加 -->
            </div>
        </div>
        
        <div class="footer">
            <p>4024俱乐部 版权所有 © 2023 | 高级抽奖系统</p>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const numberDisplay = document.getElementById('numberDisplay');
            const drawButton = document.getElementById('drawButton');
            const historyList = document.getElementById('historyList');
            
            // 创建背景粒子效果
            createParticles();
            
            // 抽奖概率（隐藏）
            const probabilities = [
                { number: 1, probability: 35 },
                { number: 2, probability: 30 },
                { number: 3, probability: 45 },
                { number: 4, probability: 40 },
                { number: 5, probability: 15 },
                { number: 6, probability: 10 }
            ];
            
            // 抽奖函数
            function drawLottery() {
                // 禁用按钮防止重复点击
                drawButton.disabled = true;
                drawButton.textContent = '抽奖中...';
                
                // 动画效果
                let counter = 0;
                const interval = setInterval(() => {
                    const randomNum = Math.floor(Math.random() * 6) + 1;
                    numberDisplay.textContent = randomNum;
                    counter++;
                    
                    if (counter > 20) {
                        clearInterval(interval);
                        
                        // 根据概率确定最终结果
                        const finalResult = getResultByProbability();
                        numberDisplay.textContent = finalResult;
                        
                        // 添加历史记录
                        addToHistory(finalResult);
                        
                        // 恢复按钮状态
                        setTimeout(() => {
                            drawButton.disabled = false;
                            drawButton.textContent = '开始抽奖';
                        }, 1000);
                    }
                }, 100);
            }
            
            // 根据概率获取结果
            function getResultByProbability() {
                const total = probabilities.reduce((sum, item) => sum + item.probability, 0);
                let random = Math.random() * total;
                
                for (let i = 0; i < probabilities.length; i++) {
                    if (random < probabilities[i].probability) {
                        return probabilities[i].number;
                    }
                    random -= probabilities[i].probability;
                }
                
                // 如果出现意外情况，返回第一个
                return probabilities[0].number;
            }
            
            // 添加历史记录
            function addToHistory(number) {
                const now = new Date();
                const timeString = now.toLocaleTimeString();
                
                const historyItem = document.createElement('div');
                historyItem.className = 'history-item';
                historyItem.innerHTML = `
                    <span class="history-number">幸运号码: ${number}</span>
                    <span class="history-time">${timeString}</span>
                `;
                
                historyList.insertBefore(historyItem, historyList.firstChild);
                
                // 限制历史记录数量
                if (historyList.children.length > 10) {
                    historyList.removeChild(historyList.lastChild);
                }
            }
            
            // 创建背景粒子
            function createParticles() {
                const particlesContainer = document.getElementById('particles');
                const particleCount = 30;
                
                for (let i = 0; i < particleCount; i++) {
                    const particle = document.createElement('div');
                    particle.className = 'particle';
                    
                    // 随机大小和位置
                    const size = Math.random() * 5 + 2;
                    const left = Math.random() * 100;
                    const animationDuration = Math.random() * 20 + 10;
                    
                    particle.style.width = `${size}px`;
                    particle.style.height = `${size}px`;
                    particle.style.left = `${left}vw`;
                    particle.style.animationDuration = `${animationDuration}s`;
                    particle.style.animationDelay = `${Math.random() * 5}s`;
                    
                    particlesContainer.appendChild(particle);
                }
            }
            
            // 绑定抽奖按钮事件
            drawButton.addEventListener('click', drawLottery);
        });
    </script>
</body>
</html>
