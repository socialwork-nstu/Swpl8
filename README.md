<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SWPL Cricket Scoreboard - Live Display</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }
        
        body {
            background: #000;
            color: white;
            min-height: 100vh;
            overflow: hidden;
        }
        
        .scoreboard-container {
            width: 100%;
            height: 100vh;
            position: relative;
            background: linear-gradient(135deg, #0a0a2a 0%, #1a1a40 100%);
        }
        
        /* হেডার সেকশন */
        .header-section {
            background: linear-gradient(90deg, #1a237e 0%, #283593 50%, #1a237e 100%);
            padding: 15px 30px;
            text-align: center;
            position: relative;
            overflow: hidden;
            border-bottom: 4px solid #ffd700;
        }
        
        .tournament-title {
            font-size: 3.5rem;
            font-weight: 900;
            letter-spacing: 3px;
            text-transform: uppercase;
            color: white;
            text-shadow: 
                0 0 10px rgba(255, 215, 0, 0.8),
                0 0 20px rgba(255, 215, 0, 0.6),
                0 0 30px rgba(255, 215, 0, 0.4);
            animation: glow 2s ease-in-out infinite alternate;
            margin-bottom: 5px;
        }
        
        @keyframes glow {
            from {
                text-shadow: 
                    0 0 10px rgba(255, 215, 0, 0.8),
                    0 0 20px rgba(255, 215, 0, 0.6),
                    0 0 30px rgba(255, 215, 0, 0.4);
            }
            to {
                text-shadow: 
                    0 0 15px rgba(255, 215, 0, 1),
                    0 0 25px rgba(255, 215, 0, 0.8),
                    0 0 35px rgba(255, 215, 0, 0.6),
                    0 0 45px rgba(255, 215, 0, 0.4);
            }
        }
        
        .tournament-subtitle {
            font-size: 1.2rem;
            font-weight: 600;
            color: #ffd700;
            letter-spacing: 1px;
        }
        
        .match-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(0, 0, 0, 0.3);
            padding: 10px 30px;
            border-bottom: 2px solid #283593;
        }
        
        .team-names {
            font-size: 2rem;
            font-weight: 700;
            color: white;
        }
        
        .vs-text {
            color: #ffd700;
            margin: 0 20px;
            font-size: 1.5rem;
        }
        
        .match-status {
            background: #4CAF50;
            color: white;
            padding: 8px 20px;
            border-radius: 20px;
            font-weight: 600;
            font-size: 1.1rem;
            animation: statusPulse 2s infinite;
        }
        
        @keyframes statusPulse {
            0% { opacity: 1; }
            50% { opacity: 0.8; }
            100% { opacity: 1; }
        }
        
        /* মেইন স্কোর সেকশন */
        .main-score-section {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 20px;
            padding: 30px;
            height: calc(100vh - 200px);
        }
        
        .left-panel {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        
        /* ✅ বিশাল রান ডিসপ্লে - টার্গেটের স্টাইল ব্যবহার */
        .big-score-display {
            background: linear-gradient(135deg, #c2185b 0%, #e91e63 100%); /* টার্গেটের ব্যাকগ্রাউন্ড */
            border-radius: 25px;
            padding: 40px 30px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.6);
            border: 5px solid #ff80ab; /* টার্গেটের বর্ডার */
            position: relative;
            overflow: hidden;
            text-align: center;
            animation: borderGlow 3s infinite alternate;
        }
        
        @keyframes borderGlow {
            0% { box-shadow: 0 0 20px rgba(255, 128, 171, 0.5); }
            100% { box-shadow: 0 0 40px rgba(255, 128, 171, 0.8), 0 0 60px rgba(255, 128, 171, 0.4); }
        }
        
        .big-score-title {
            font-size: 2.5rem;
            color: white; /* সাদা কালার */
            margin-bottom: 20px;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 2px;
        }
        
        /* ✅ বিশাল সাইজের রান নাম্বার - বোল্ড ও ছায়া ছাড়া */
        .big-score-value {
            font-size: 10rem;
            font-weight: 900;
            color: white; /* সাদা কালার */
            line-height: 0.9;
            margin-bottom: 10px;
            animation: scorePulse 0.5s ease-out;
            text-shadow: none !important; /* টেক্সট শ্যাডো রিমুভ */
            font-weight: 900; /* এক্সট্রা বোল্ড */
            letter-spacing: -2px;
        }
        
        @keyframes scorePulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .big-score-wickets {
            font-size: 3rem;
            font-weight: 800;
            color: white; /* সাদা কালার */
            margin-bottom: 15px;
            opacity: 0.9;
        }
        
        .big-score-overs {
            font-size: 2.5rem;
            font-weight: 700;
            color: white; /* সাদা কালার */
            margin-bottom: 10px;
            opacity: 0.9;
        }
        
        .big-score-runrate {
            font-size: 2rem;
            font-weight: 600;
            color: white; /* সাদা কালার */
            background: rgba(255, 255, 255, 0.2); /* হালকা সাদা ব্যাকগ্রাউন্ড */
            padding: 10px 20px;
            border-radius: 15px;
            display: inline-block;
        }
        
        .wickets-badge {
            position: absolute;
            top: 20px;
            right: 20px;
            background: #ff5252;
            color: white;
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 1.5rem;
            font-weight: 700;
            animation: badgePulse 1.5s infinite;
        }
        
        @keyframes badgePulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }
        
        .innings-badge {
            position: absolute;
            top: 20px;
            left: 20px;
            background: rgba(255, 255, 255, 0.2); /* হালকা সাদা */
            color: white;
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 1.5rem;
            font-weight: 700;
        }
        
        /* ✅ টার্গেট ডিসপ্লে - নতুন কালার */
        .target-display-box {
            background: linear-gradient(135deg, #0d47a1 0%, #1565c0 100%); /* নতুন নীল গ্রেডিয়েন্ট */
            border-radius: 20px;
            padding: 25px;
            text-align: center;
            box-shadow: 0 10px 25px rgba(13, 71, 161, 0.4);
            border: 3px solid #64b5f6; /* হালকা নীল বর্ডার */
            animation: targetPulse 2s infinite alternate;
        }
        
        @keyframes targetPulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.02); }
        }
        
        .target-title {
            font-size: 2rem;
            color: white;
            margin-bottom: 15px;
            font-weight: 800;
        }
        
        .target-value {
            font-size: 5rem;
            font-weight: 900;
            color: #bbdefb; /* হালকা নীল কালার */
            text-shadow: 3px 3px 5px rgba(0,0,0,0.7);
            margin-bottom: 15px;
        }
        
        /* ✅ টার্গেট ইনফো - নতুন কালারফুল ডিজাইন */
        .target-info {
            font-size: 1.8rem; /* আরও বড় করা */
            color: white;
            background: linear-gradient(135deg, #ff9800 0%, #ff5722 100%); /* কমলা-লাল গ্রেডিয়েন্ট */
            padding: 15px 20px;
            border-radius: 15px;
            font-weight: 800;
            margin-bottom: 12px;
            box-shadow: 0 5px 15px rgba(255, 87, 34, 0.3);
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
            animation: targetInfoPulse 2s infinite alternate;
        }
        
        @keyframes targetInfoPulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.03); }
        }
        
        /* ✅ টার্গেট ডিটেইলস - নতুন কালারফুল ডিজাইন */
        .target-details {
            font-size: 2.5rem; /* আরও বড় করা */
            font-weight: 900;
            color: white;
            background: linear-gradient(135deg, #4CAF50 0%, #2E7D32 100%); /* সবুজ গ্রেডিয়েন্ট */
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 8px 20px rgba(76, 175, 80, 0.4);
            text-align: center;
            margin-top: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
            animation: targetDetailsGlow 2s infinite alternate;
            border: 3px solid #81c784;
        }
        
        @keyframes targetDetailsGlow {
            0% {
                box-shadow: 0 8px 20px rgba(76, 175, 80, 0.4),
                            0 0 10px rgba(76, 175, 80, 0.2);
            }
            100% {
                box-shadow: 0 8px 25px rgba(76, 175, 80, 0.6),
                            0 0 20px rgba(76, 175, 80, 0.4),
                            0 0 30px rgba(76, 175, 80, 0.2);
            }
        }
        
        /* ✅ টার্গেট স্ট্যাটাস - আরও আকর্ষণীয় */
        .target-status {
            font-size: 1.8rem;
            color: #ffd700;
            background: rgba(0, 0, 0, 0.3);
            padding: 12px 20px;
            border-radius: 12px;
            font-weight: 700;
            margin-top: 15px;
            border: 2px solid #ffd700;
            animation: statusShine 3s infinite;
        }
        
        @keyframes statusShine {
            0%, 100% { box-shadow: 0 0 5px rgba(255, 215, 0, 0.3); }
            50% { box-shadow: 0 0 15px rgba(255, 215, 0, 0.6); }
        }
        
        /* ম্যাচ রেজাল্ট স্টাইল */
        .match-result-display {
            background: linear-gradient(135deg, #ff9800 0%, #ff5722 100%);
            border-radius: 20px;
            padding: 25px;
            text-align: center;
            box-shadow: 0 10px 25px rgba(255, 87, 34, 0.4);
            border: 3px solid #ffcc80;
            animation: resultPulse 1s infinite alternate;
        }
        
        @keyframes resultPulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.03); }
        }
        
        .result-title {
            font-size: 2.5rem;
            color: white;
            margin-bottom: 15px;
            font-weight: 800;
            text-transform: uppercase;
        }
        
        .result-winner {
            font-size: 3.5rem;
            font-weight: 900;
            color: #ffd700;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
        
        .result-margin {
            font-size: 2rem;
            color: white;
            margin-bottom: 15px;
        }
        
        /* প্লেয়ার সেকশন */
        .players-section {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 20px;
        }
        
        .player-box {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 20px;
            border: 2px solid rgba(255, 215, 0, 0.2);
        }
        
        .player-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid rgba(255, 215, 0, 0.3);
        }
        
        .player-title {
            font-size: 1.2rem;
            color: #ffd700;
            font-weight: 700;
        }
        
        .player-name {
            font-size: 1.8rem;
            font-weight: 700;
            color: white;
            margin-bottom: 15px;
        }
        
        .player-stats {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            text-align: center;
        }
        
        .stat-item {
            background: rgba(0, 0, 0, 0.3);
            padding: 10px;
            border-radius: 10px;
        }
        
        .stat-label {
            font-size: 0.9rem;
            color: #aaa;
            margin-bottom: 5px;
        }
        
        .stat-value {
            font-size: 1.3rem;
            font-weight: 700;
            color: white;
        }
        
        .striker-indicator {
            background: #ff5722;
            color: white;
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.9rem;
            font-weight: 700;
            animation: strikerBlink 1s infinite;
        }
        
        @keyframes strikerBlink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }
        
        /* রাইট প্যানেল */
        .right-panel {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        
        /* বোলার সেকশন */
        .bowler-section {
            background: linear-gradient(135deg, #00838f 0%, #0097a7 100%);
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 10px 20px rgba(0, 131, 143, 0.3);
        }
        
        .bowler-title {
            font-size: 1.5rem;
            color: white;
            margin-bottom: 20px;
            text-align: center;
            font-weight: 700;
        }
        
        .bowler-name {
            font-size: 2rem;
            font-weight: 700;
            color: white;
            text-align: center;
            margin-bottom: 20px;
        }
        
        .bowler-info {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            text-align: center;
        }
        
        .bowler-stat {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
        }
        
        .bowler-stat-label {
            font-size: 0.9rem;
            color: rgba(255, 255, 255, 0.8);
            margin-bottom: 5px;
        }
        
        .bowler-stat-value {
            font-size: 1.5rem;
            font-weight: 700;
            color: white;
        }
        
        /* শেষ ৬ বলের ডিসপ্লে */
        .recent-balls-section {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 20px;
            padding: 25px;
            border: 2px solid rgba(255, 215, 0, 0.2);
        }
        
        .recent-balls-title {
            font-size: 1.5rem;
            color: #ffd700;
            margin-bottom: 20px;
            text-align: center;
            font-weight: 700;
        }
        
        .recent-balls-container {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 10px;
            text-align: center;
        }
        
        .ball-item {
            background: rgba(0, 0, 0, 0.3);
            padding: 15px 5px;
            border-radius: 10px;
            border: 2px solid transparent;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }
        
        .ball-item.latest {
            border-color: #4CAF50;
            animation: ballPulse 1s infinite;
        }
        
        @keyframes ballPulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        
        .ball-item.wicket {
            background: linear-gradient(135deg, #d32f2f 0%, #f44336 100%);
        }
        
        .ball-item.four {
            background: linear-gradient(135deg, #3949ab 0%, #3f51b5 100%);
        }
        
        .ball-item.six {
            background: linear-gradient(135deg, #c2185b 0%, #e91e63 100%);
        }
        
        .ball-item.wide {
            background: linear-gradient(135deg, #1976d2 0%, #2196f3 100%);
        }
        
        .ball-item.noball {
            background: linear-gradient(135deg, #f57c00 0%, #ff9800 100%);
        }
        
        .ball-number {
            position: absolute;
            top: 5px;
            left: 5px;
            font-size: 0.7rem;
            color: rgba(255, 255, 255, 0.7);
        }
        
        .ball-value {
            font-size: 1.8rem;
            font-weight: 800;
            color: white;
            margin-bottom: 5px;
        }
        
        .ball-type {
            font-size: 0.8rem;
            color: rgba(255, 255, 255, 0.8);
            text-transform: uppercase;
        }
        
        /* রিসেন্ট অ্যাকটিভিটি */
        .recent-activity {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 20px;
            padding: 25px;
            flex-grow: 1;
            border: 2px solid rgba(255, 215, 0, 0.2);
        }
        
        .activity-title {
            font-size: 1.5rem;
            color: #ffd700;
            margin-bottom: 20px;
            text-align: center;
            font-weight: 700;
        }
        
        .activity-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
            max-height: 200px;
            overflow-y: auto;
            padding-right: 10px;
        }
        
        .activity-item {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-left: 4px solid #4CAF50;
            animation: slideIn 0.3s ease;
        }
        
        @keyframes slideIn {
            from { transform: translateX(-100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
        
        .activity-runs {
            font-size: 1.2rem;
            font-weight: 700;
            color: #4CAF50;
        }
        
        .activity-player {
            font-size: 0.9rem;
            color: #aaa;
        }
        
        /* ফুটার */
        .footer-section {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(0, 0, 0, 0.5);
            padding: 10px 30px;
            text-align: center;
            border-top: 2px solid #283593;
        }
        
        .last-updated {
            font-size: 0.9rem;
            color: #aaa;
        }
        
        /* রেসপন্সিভ ডিজাইন */
        @media (max-width: 1200px) {
            .main-score-section {
                grid-template-columns: 1fr;
                height: auto;
            }
            
            .tournament-title {
                font-size: 2.5rem;
            }
            
            .big-score-value {
                font-size: 8rem;
            }
            
            .target-details {
                font-size: 2.2rem;
                padding: 18px;
            }
        }
        
        @media (max-width: 768px) {
            .tournament-title {
                font-size: 2rem;
            }
            
            .team-names {
                font-size: 1.5rem;
            }
            
            .players-section {
                grid-template-columns: 1fr;
            }
            
            .big-score-value {
                font-size: 6rem;
            }
            
            .target-value {
                font-size: 4rem;
            }
            
            .target-details {
                font-size: 1.8rem;
                padding: 15px;
            }
            
            .target-info {
                font-size: 1.5rem;
                padding: 12px 15px;
            }
            
            .recent-balls-container {
                grid-template-columns: repeat(3, 1fr);
            }
        }
        
        @media (max-width: 480px) {
            .tournament-title {
                font-size: 1.5rem;
            }
            
            .big-score-value {
                font-size: 4rem;
            }
            
            .target-value {
                font-size: 3rem;
            }
            
            .target-details {
                font-size: 1.5rem;
                padding: 12px;
            }
            
            .target-info {
                font-size: 1.3rem;
                padding: 10px 12px;
            }
            
            .recent-balls-container {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="scoreboard-container">
        <!-- হেডার সেকশন -->
        <div class="header-section">
            <div class="tournament-title">
                SOCIAL WORK PREMIER LEAGUE - SWPL
            </div>
            <div class="tournament-subtitle">
                LIVE CRICKET SCOREBOARD
            </div>
        </div>
        
        <!-- ম্যাচ ইনফো -->
        <div class="match-info">
            <div class="team-names">
                <span id="team1Display">TEAM A</span>
                <span class="vs-text">VS</span>
                <span id="team2Display">TEAM B</span>
            </div>
            <div class="match-status" id="matchStatusDisplay">MATCH STARTING SOON</div>
        </div>
        
        <!-- মেইন স্কোর সেকশন -->
        <div class="main-score-section">
            <!-- লেফট প্যানেল -->
            <div class="left-panel">
                <!-- ✅ বিশাল রান ডিসপ্লে -->
                <div class="big-score-display">
                    <div class="innings-badge" id="inningsBadge">1st INNINGS</div>
                    <div class="wickets-badge" id="wicketsBadge">0 WICKETS</div>
                    
                    <div class="big-score-title">TOTAL SCORE</div>
                    <div class="big-score-value" id="totalRunsDisplay">0</div>
                    <div class="big-score-wickets" id="totalWicketsDisplay">for 0</div>
                    <div class="big-score-overs" id="totalOversDisplay">0.0 overs</div>
                    <div class="big-score-runrate" id="runRateDisplay">Run Rate: 0.00</div>
                </div>
                
                <!-- ✅ টার্গেট ডিসপ্লে (২য় ইনিংসে দেখা যাবে) -->
                <div class="target-display-box" id="targetDisplayBox" style="display: none;">
                    <div class="target-title">TARGET TO WIN</div>
                    <div class="target-value" id="targetDisplay">-</div>
                    
                    <div class="target-info" id="targetInfoDisplay">1st innings in progress</div>
                    
                    <!-- ✅ নতুন কালারফুল টার্গেট ডিটেইলস -->
                    <div class="target-details" id="targetDetailsDisplay">
                        <!-- এখানে "Need X runs in Y balls" ডাইনামিকভাবে দেখানো হবে -->
                    </div>
                    
                    <div class="target-status" id="targetStatusDisplay">
                        <!-- এখানে অতিরিক্ত টার্গেট স্ট্যাটাস দেখানো হবে -->
                    </div>
                </div>
                
                <!-- ✅ ম্যাচ রেজাল্ট (ম্যাচ শেষ হলে দেখা যাবে) -->
                <div class="match-result-display" id="matchResultBox" style="display: none;">
                    <div class="result-title" id="resultTitle">MATCH RESULT</div>
                    <div class="result-winner" id="resultWinnerDisplay">-</div>
                    <div class="result-margin" id="resultMarginDisplay">-</div>
                </div>
                
                <!-- প্লেয়ার সেকশন -->
                <div class="players-section">
                    <!-- ব্যাটসম্যান ১ -->
                    <div class="player-box">
                        <div class="player-header">
                            <div class="player-title">BATSMAN 1</div>
                            <div class="striker-indicator" id="striker1Indicator">STRIKER</div>
                        </div>
                        <div class="player-name" id="batsman1NameDisplay">PLAYER 1</div>
                        <div class="player-stats">
                            <div class="stat-item">
                                <div class="stat-label">RUNS</div>
                                <div class="stat-value" id="batsman1RunsDisplay">0</div>
                            </div>
                            <div class="stat-item">
                                <div class="stat-label">BALLS</div>
                                <div class="stat-value" id="batsman1BallsDisplay">0</div>
                            </div>
                            <div class="stat-item">
                                <div class="stat-label">S/R</div>
                                <div class="stat-value" id="batsman1SRDisplay">0.00</div>
                            </div>
                        </div>
                    </div>
                    
                    <!-- ব্যাটসম্যান ২ -->
                    <div class="player-box">
                        <div class="player-header">
                            <div class="player-title">BATSMAN 2</div>
                            <div class="striker-indicator" id="striker2Indicator" style="display: none;">STRIKER</div>
                        </div>
                        <div class="player-name" id="batsman2NameDisplay">PLAYER 2</div>
                        <div class="player-stats">
                            <div class="stat-item">
                                <div class="stat-label">RUNS</div>
                                <div class="stat-value" id="batsman2RunsDisplay">0</div>
                            </div>
                            <div class="stat-item">
                                <div class="stat-label">BALLS</div>
                                <div class="stat-value" id="batsman2BallsDisplay">0</div>
                            </div>
                            <div class="stat-item">
                                <div class="stat-label">S/R</div>
                                <div class="stat-value" id="batsman2SRDisplay">0.00</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- রাইট প্যানেল -->
            <div class="right-panel">
                <!-- বোলার সেকশন -->
                <div class="bowler-section">
                    <div class="bowler-title">CURRENT BOWLER</div>
                    <div class="bowler-name" id="bowlerNameDisplay">BOWLER 1</div>
                    <div class="bowler-info">
                        <div class="bowler-stat">
                            <div class="bowler-stat-label">OVERS</div>
                            <div class="bowler-stat-value" id="bowlerOversDisplay">0.0</div>
                        </div>
                        <div class="bowler-stat">
                            <div class="bowler-stat-label">RUNS</div>
                            <div class="bowler-stat-value" id="bowlerRunsDisplay">0</div>
                        </div>
                        <div class="bowler-stat">
                            <div class="bowler-stat-label">WICKETS</div>
                            <div class="bowler-stat-value" id="bowlerWicketsDisplay">0</div>
                        </div>
                        <div class="bowler-stat">
                            <div class="bowler-stat-label">ECON</div>
                            <div class="bowler-stat-value" id="bowlerEconDisplay">0.00</div>
                        </div>
                    </div>
                </div>
                
                <!-- শেষ ৬ বলের ডিসপ্লে -->
                <div class="recent-balls-section">
                    <div class="recent-balls-title">LAST 6 BALLS</div>
                    <div class="recent-balls-container" id="recentBallsContainer">
                        <!-- বলগুলো ডায়নামিকভাবে লোড হবে -->
                        <div class="ball-item">
                            <div class="ball-number">1</div>
                            <div class="ball-value">-</div>
                            <div class="ball-type">-</div>
                        </div>
                        <div class="ball-item">
                            <div class="ball-number">2</div>
                            <div class="ball-value">-</div>
                            <div class="ball-type">-</div>
                        </div>
                        <div class="ball-item">
                            <div class="ball-number">3</div>
                            <div class="ball-value">-</div>
                            <div class="ball-type">-</div>
                        </div>
                        <div class="ball-item">
                            <div class="ball-number">4</div>
                            <div class="ball-value">-</div>
                            <div class="ball-type">-</div>
                        </div>
                        <div class="ball-item">
                            <div class="ball-number">5</div>
                            <div class="ball-value">-</div>
                            <div class="ball-type">-</div>
                        </div>
                        <div class="ball-item">
                            <div class="ball-number">6</div>
                            <div class="ball-value">-</div>
                            <div class="ball-type">-</div>
                        </div>
                    </div>
                </div>
                
                <!-- রিসেন্ট অ্যাকটিভিটি -->
                <div class="recent-activity">
                    <div class="activity-title">RECENT ACTIVITY</div>
                    <div class="activity-list" id="activityList">
                        <div class="activity-item">
                            <span class="activity-runs">Match Started</span>
                            <span class="activity-player">Ready</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- ফুটার -->
        <div class="footer-section">
            <div class="last-updated" id="lastUpdatedDisplay">
                Last Updated: <span id="lastUpdatedTime">00:00:00</span>
            </div>
        </div>
    </div>

    <script>
        // ডিসপ্লে ডেটা
        let displayData = {
            team1: "TEAM A",
            team2: "TEAM B",
            matchStatus: "MATCH STARTING SOON",
            innings: 1,
            totalRuns: 0,
            totalWickets: 0,
            totalOvers: "0.0",
            totalOversLimit: 20,
            runRate: "0.00",
            target: null,
            targetInfo: "1st innings in progress",
            matchResult: null,
            
            batsman1: {
                name: "PLAYER 1",
                runs: 0,
                balls: 0,
                strikerate: 0.00,
                isStriker: true
            },
            batsman2: {
                name: "PLAYER 2",
                runs: 0,
                balls: 0,
                strikerate: 0.00,
                isStriker: false
            },
            bowler: {
                name: "BOWLER 1",
                overs: "0.0",
                runs: 0,
                wickets: 0,
                economy: "0.00"
            },
            
            recentBalls: [],
            recentActivity: [
                { runs: "Match Started", player: "Ready" }
            ],
            
            lastUpdated: "00:00:00"
        };

        // ✅ ডিসপ্লে আপডেট ফাংশন
        function updateDisplay() {
            try {
                const savedData = localStorage.getItem('cricketMatchData');
                if (savedData) {
                    const matchData = JSON.parse(savedData);
                    
                    // টিম নাম
                    document.getElementById('team1Display').textContent = matchData.team1;
                    document.getElementById('team2Display').textContent = matchData.team2;
                    
                    // ম্যাচ স্ট্যাটাস
                    document.getElementById('matchStatusDisplay').textContent = matchData.matchStatus;
                    
                    // ✅ বিশাল রান ডিসপ্লে এনিমেশন
                    const runsElement = document.getElementById('totalRunsDisplay');
                    const oldRuns = parseInt(runsElement.textContent) || 0;
                    const newRuns = matchData.totalRuns;
                    
                    if (oldRuns !== newRuns) {
                        runsElement.style.animation = 'none';
                        setTimeout(() => {
                            runsElement.style.animation = 'scorePulse 0.5s ease-out';
                        }, 10);
                    }
                    runsElement.textContent = newRuns;
                    
                    // উইকেটস
                    document.getElementById('totalWicketsDisplay').textContent = `for ${matchData.totalWickets}`;
                    document.getElementById('wicketsBadge').textContent = `${matchData.totalWickets} WICKET${matchData.totalWickets !== 1 ? 'S' : ''}`;
                    
                    // ইনিংস ব্যাজ
                    document.getElementById('inningsBadge').textContent = matchData.innings === 1 ? "1st INNINGS" : "2nd INNINGS";
                    
                    // মোট ওভার
                    const totalBalls = matchData.bowlersList.reduce((sum, bowler) => sum + bowler.balls, 0);
                    const fullOvers = Math.floor(totalBalls / 6);
                    const remainingBalls = totalBalls % 6;
                    const totalOvers = parseFloat(fullOvers + (remainingBalls / 10)).toFixed(1);
                    document.getElementById('totalOversDisplay').textContent = `${totalOvers} overs`;
                    
                    // রান রেট
                    const totalOversDecimal = totalBalls / 6;
                    const runRate = totalOversDecimal > 0 ? (matchData.totalRuns / totalOversDecimal).toFixed(2) : "0.00";
                    document.getElementById('runRateDisplay').textContent = `Run Rate: ${runRate}`;
                    
                    // ✅ টার্গেট ডিসপ্লে (২য় ইনিংসে)
                    const targetBox = document.getElementById('targetDisplayBox');
                    const resultBox = document.getElementById('matchResultBox');
                    const targetDetails = document.getElementById('targetDetailsDisplay');
                    const targetStatus = document.getElementById('targetStatusDisplay');
                    
                    if (matchData.matchStatus === "MATCH ENDED" && matchData.matchResult) {
                        // ম্যাচ রেজাল্ট দেখানো
                        targetBox.style.display = 'none';
                        resultBox.style.display = 'block';
                        
                        document.getElementById('resultWinnerDisplay').textContent = matchData.matchResult.winner;
                        document.getElementById('resultMarginDisplay').textContent = 
                            `Won by ${matchData.matchResult.winBy}`;
                    } else if (matchData.innings === 2 && matchData.target) {
                        // ২য় ইনিংস টার্গেট দেখানো
                        targetBox.style.display = 'block';
                        resultBox.style.display = 'none';
                        
                        document.getElementById('targetDisplay').textContent = matchData.target;
                        
                        const ballsRemaining = (matchData.totalOvers * 6) - totalBalls;
                        const runsNeeded = matchData.target - matchData.totalRuns;
                        const oversRemaining = (ballsRemaining / 6).toFixed(1);
                        
                        // ✅ নতুন কালারফুল টার্গেট ইনফো
                        if (runsNeeded > 0) {
                            // টার্গেট ইনফো - কমলা-লাল ব্যাকগ্রাউন্ড
                            document.getElementById('targetInfoDisplay').textContent = 
                                `Chasing ${matchData.target} runs`;
                            
                            // ✅ নতুন কালারফুল টার্গেট ডিটেইলস - বড় সাইজ, সবুজ ব্যাকগ্রাউন্ড
                            targetDetails.innerHTML = `
                                <div style="font-size: 2.8rem; margin-bottom: 5px;">
                                    <span style="color: #ffeb3b;">NEED</span> 
                                    <span style="color: #fff; text-shadow: 2px 2px 4px rgba(0,0,0,0.7);">${runsNeeded}</span> 
                                    <span style="color: #ffeb3b;">RUNS</span>
                                </div>
                                <div style="font-size: 2.2rem;">
                                    <span style="color: #81c784;">IN</span> 
                                    <span style="color: #fff; text-shadow: 2px 2px 4px rgba(0,0,0,0.7);">${ballsRemaining}</span> 
                                    <span style="color: #81c784;">BALLS</span>
                                    <span style="color: #bbdefb;">(${oversRemaining} overs)</span>
                                </div>
                            `;
                            
                            // টার্গেট স্ট্যাটাস
                            const requiredRunRate = ballsRemaining > 0 ? (runsNeeded / (ballsRemaining / 6)).toFixed(2) : "∞";
                            targetStatus.innerHTML = `
                                <span style="color: #ffd700;">Required Run Rate: </span>
                                <span style="color: #fff; font-weight: 800;">${requiredRunRate}</span>
                            `;
                            
                            // টার্গেট ডিটেইলস বক্স দেখা যাবে
                            targetDetails.style.display = 'block';
                            targetStatus.style.display = 'block';
                        } else {
                            // ম্যাচ জিতেছে
                            document.getElementById('targetInfoDisplay').textContent = 
                                `TARGET ACHIEVED!`;
                            
                            // ✅ নতুন কালারফুল টার্গেট ডিটেইলস - জয়লের বার্তা
                            targetDetails.innerHTML = `
                                <div style="font-size: 3rem; color: #ffd700; text-shadow: 2px 2px 8px rgba(255, 215, 0, 0.7);">
                                    🏆 MATCH WON! 🏆
                                </div>
                                <div style="font-size: 2.2rem; margin-top: 10px; color: #fff;">
                                    ${matchData.totalRuns}/${matchData.totalWickets}
                                </div>
                            `;
                            
                            // টার্গেট ডিটেইলসের ব্যাকগ্রাউন্ড পরিবর্তন
                            targetDetails.style.background = 'linear-gradient(135deg, #ff9800 0%, #ff5722 100%)';
                            targetDetails.style.border = '3px solid #ffcc80';
                            
                            targetStatus.innerHTML = `
                                <span style="color: #4CAF50;">Congratulations!</span>
                            `;
                            
                            // টার্গেট ডিটেইলস বক্স দেখা যাবে
                            targetDetails.style.display = 'block';
                            targetStatus.style.display = 'block';
                        }
                    } else {
                        // প্রথম ইনিংস বা অন্য স্ট্যাটাস
                        targetBox.style.display = 'none';
                        resultBox.style.display = 'none';
                        targetDetails.style.display = 'none';
                        targetStatus.style.display = 'none';
                    }
                    
                    // ব্যাটসম্যান ১
                    document.getElementById('batsman1NameDisplay').textContent = matchData.batsman1.name;
                    document.getElementById('batsman1RunsDisplay').textContent = matchData.batsman1.runs;
                    document.getElementById('batsman1BallsDisplay').textContent = matchData.batsman1.balls;
                    document.getElementById('batsman1SRDisplay').textContent = matchData.batsman1.strikerate.toFixed(2);
                    
                    // ব্যাটসম্যান ২
                    document.getElementById('batsman2NameDisplay').textContent = matchData.batsman2.name;
                    document.getElementById('batsman2RunsDisplay').textContent = matchData.batsman2.runs;
                    document.getElementById('batsman2BallsDisplay').textContent = matchData.batsman2.balls;
                    document.getElementById('batsman2SRDisplay').textContent = matchData.batsman2.strikerate.toFixed(2);
                    
                    // স্ট্রাইকার ইন্ডিকেটর
                    if (matchData.batsman1.isStriker) {
                        document.getElementById('striker1Indicator').style.display = 'block';
                        document.getElementById('striker2Indicator').style.display = 'none';
                    } else {
                        document.getElementById('striker1Indicator').style.display = 'none';
                        document.getElementById('striker2Indicator').style.display = 'block';
                    }
                    
                    // বোলার
                    document.getElementById('bowlerNameDisplay').textContent = matchData.bowler.name;
                    document.getElementById('bowlerOversDisplay').textContent = matchData.bowler.overs;
                    document.getElementById('bowlerRunsDisplay').textContent = matchData.bowler.runs;
                    document.getElementById('bowlerWicketsDisplay').textContent = matchData.bowler.wickets;
                    document.getElementById('bowlerEconDisplay').textContent = matchData.bowler.economy.toFixed(2);
                    
                    // ✅ শেষ ৬ বলের ডিসপ্লে আপডেট
                    updateRecentBalls(matchData.recentBalls || []);
                    
                    // রিসেন্ট অ্যাকটিভিটি
                    updateActivityList(matchData.runLog || []);
                    
                    // লাস্ট আপডেটেড
                    document.getElementById('lastUpdatedTime').textContent = matchData.lastUpdated || new Date().toLocaleTimeString();
                }
            } catch (e) {
                console.log("Error loading display data:", e);
            }
        }
        
        // ✅ ফাংশন: শেষ ৬ বলের ডিসপ্লে আপডেট
        function updateRecentBalls(recentBalls) {
            const container = document.getElementById('recentBallsContainer');
            container.innerHTML = '';
            
            const ballsToShow = [...recentBalls].reverse().slice(0, 6);
            
            if (ballsToShow.length === 0) {
                for (let i = 1; i <= 6; i++) {
                    const ballItem = document.createElement('div');
                    ballItem.className = 'ball-item';
                    ballItem.innerHTML = `
                        <div class="ball-number">${i}</div>
                        <div class="ball-value">-</div>
                        <div class="ball-type">-</div>
                    `;
                    container.appendChild(ballItem);
                }
                return;
            }
            
            // বলগুলো দেখানো
            ballsToShow.forEach((ball, index) => {
                const ballItem = document.createElement('div');
                ballItem.className = 'ball-item';
                
                // বলের ধরন অনুযায়ী ক্লাস যোগ
                if (ball === 'W') {
                    ballItem.classList.add('wicket');
                } else if (ball === '4') {
                    ballItem.classList.add('four');
                } else if (ball === '6') {
                    ballItem.classList.add('six');
                } else if (ball.startsWith('WD')) {
                    ballItem.classList.add('wide');
                } else if (ball.startsWith('NB')) {
                    ballItem.classList.add('noball');
                }
                
                // সর্বশেষ বল হাইলাইট
                if (index === 0) {
                    ballItem.classList.add('latest');
                }
                
                let ballValue = ball;
                let ballType = "run";
                
                if (ball === 'W') {
                    ballValue = 'W';
                    ballType = 'wicket';
                } else if (ball === '0') {
                    ballValue = '0';
                    ballType = 'dot';
                } else if (ball.startsWith('WD')) {
                    ballValue = ball.replace('WD+', '');
                    ballType = 'wide';
                } else if (ball.startsWith('NB')) {
                    ballValue = ball.replace('NB+', '');
                    ballType = 'noball';
                } else {
                    ballType = 'run';
                }
                
                ballItem.innerHTML = `
                    <div class="ball-number">${ballsToShow.length - index}</div>
                    <div class="ball-value">${ballValue}</div>
                    <div class="ball-type">${ballType}</div>
                `;
                
                container.appendChild(ballItem);
            });
            
            // বাকি জায়গা ফাকা রাখা
            const emptySlots = 6 - ballsToShow.length;
            for (let i = 1; i <= emptySlots; i++) {
                const ballItem = document.createElement('div');
                ballItem.className = 'ball-item';
                ballItem.innerHTML = `
                    <div class="ball-number">${ballsToShow.length + i}</div>
                    <div class="ball-value">-</div>
                    <div class="ball-type">-</div>
                `;
                container.appendChild(ballItem);
            }
        }
        
        // ✅ ফাংশন: অ্যাকটিভিটি লিস্ট আপডেট
        function updateActivityList(runLog) {
            const activityList = document.getElementById('activityList');
            activityList.innerHTML = '';
            
            const recentLogs = runLog.slice(-8).reverse();
            
            if (recentLogs.length === 0) {
                const emptyItem = document.createElement('div');
                emptyItem.className = 'activity-item';
                emptyItem.innerHTML = `
                    <span class="activity-runs">No activity yet</span>
                    <span class="activity-player">-</span>
                `;
                activityList.appendChild(emptyItem);
                return;
            }
            
            recentLogs.forEach(log => {
                const activityItem = document.createElement('div');
                activityItem.className = 'activity-item';
                
                let runsText = "";
                if (log.isWicket) {
                    runsText = 'Wicket';
                } else if (log.runs === 0 && log.extraType === 'none') {
                    runsText = 'Dot ball';
                } else if (log.runs > 0) {
                    runsText = `${log.runs} run${log.runs !== 1 ? 's' : ''}`;
                } else if (log.extraType === 'wide') {
                    runsText = `Wide +${log.extraRuns}`;
                } else if (log.extraType === 'noball') {
                    runsText = `No ball +${log.extraRuns}`;
                } else if (log.extraType === 'bye') {
                    runsText = `Bye +${log.runs || 1}`;
                } else if (log.extraType === 'legbye') {
                    runsText = `Leg bye +${log.runs || 1}`;
                } else {
                    runsText = `${log.extraType}`;
                }
                
                activityItem.innerHTML = `
                    <span class="activity-runs">${runsText}</span>
                    <span class="activity-player">${log.batsman}</span>
                `;
                
                activityList.appendChild(activityItem);
            });
        }
        
        // পেজ লোড হলে এবং নিয়মিত আপডেট
        document.addEventListener('DOMContentLoaded', function() {
            // প্রথম আপডেট
            updateDisplay();
            
            // প্রতি সেকেন্ডে আপডেট চেক
            setInterval(updateDisplay, 1000);
            
            // কীবোর্ড শর্টকাট
            document.addEventListener('keydown', function(e) {
                if (e.key === 'F5') {
                    e.preventDefault();
                    updateDisplay();
                    console.log("Display manually refreshed");
                }
            });
        });
    </script>
</body>
</html>
