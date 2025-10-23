# cyicyi09.github.io-
비행 공부 어플
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flight Study App</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;700&family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        /* --- 1. 기본 & 글꼴 설정 --- */
        :root {
            --primary-color: #007AFF; /* Apple Blue */
            --secondary-color: #FF9500; /* Apple Orange */
            --bg-color: #f4f5f7;
            --card-bg: #fff;
            --text-color: #1c1c1e;
            --subtext-color: #8a8a8e;
            --border-color: #e5e5ea;
            --red-color: #FF3B30;
            --gradient-start: #1c2541; /* 시작 화면 그라데이션 */
            --gradient-end: #0b132b;
        }
        body {
            font-family: 'Roboto', 'Noto Sans KR', sans-serif;
            background-color: #333; /* 앱 바깥 배경 */
            color: var(--text-color);
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            text-align: center;
            overflow: hidden; 
            -webkit-font-smoothing: antialiased; /* 글꼴 부드럽게 */
            -moz-osx-font-smoothing: grayscale;
        }

        /* --- [UI 개편] 앱 컨테이너 (Toss/Apple 스타일) --- */
        .app-container {
            width: 100%;
            height: 100%;
            max-width: 500px; 
            max-height: 900px; 
            background: var(--bg-color); 
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            overflow: hidden;
            position: relative; /* 모든 화면의 기준 */
        }

        /* --- 2. 화면 공통 스타일 --- */
        .screen {
            display: none; 
            flex-direction: column;
            justify-content: flex-start; 
            align-items: center;
            position: absolute; 
            width: 100%;
            height: 100%;
            padding: 0; 
            box-sizing: border-box;
            animation: fadeIn 0.3s ease-out; /* 애니메이션 부드럽게 */
            background-color: var(--bg-color);
            opacity: 0; /* 기본 숨김 */
            visibility: hidden; /* 접근성 위해 추가 */
            transition: opacity 0.3s, visibility 0.3s;
        }
        .screen.active {
            display: flex; 
            opacity: 1;
            visibility: visible;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); } 
            to { opacity: 1; transform: translateY(0); }
        }

        h1 { font-size: 3em; font-weight: 700; color: white; margin-bottom: 0; }
        h2 { font-size: 1.5em; font-weight: 700; color: var(--text-color); margin: 0;}
        p { font-size: 1em; color: var(--subtext-color); }
        p.subtitle { color: #888; font-size: 1em; margin-top: 5px; }

        /* [UI 개편] 헤더 스타일 */
        .header {
            width: 100%;
            padding: 15px 20px; /* 패딩 조정 */
            box-sizing: border-box;
            display: flex;
            align-items: center;
            position: relative;
            justify-content: center; 
            background-color: var(--bg-color); 
            z-index: 10;
            border-bottom: 1px solid var(--border-color); /* 구분선 */
        }
        .header .back-button {
            position: absolute;
            left: 15px;
            top: 50%;
            transform: translateY(-50%);
            margin: 0;
        }
        .content-area {
            width: 100%;
            padding: 20px; /* 좌우 패딩 추가 */
            box-sizing: border-box;
            display: flex;
            flex-direction: column;
            align-items: center;
            flex-grow: 1; /* 남은 공간 채우기 */
        }
        .content-area.scrollable {
            overflow-y: auto; /* 스크롤 필요한 부분 */
            -webkit-overflow-scrolling: touch; /* iOS 부드러운 스크롤 */
            padding-bottom: 80px; /* 하단 여백 추가 */
        }
        .content-area.column {
            justify-content: center; /* 세로 중앙 정렬 */
        }

        /* [UI 개편] 버튼 스타일 (Toss/Apple) */
        button {
            cursor: pointer;
            border: none;
            transition: all 0.2s ease;
            font-family: 'Noto Sans KR', sans-serif;
            font-weight: 700;
        }
        button:disabled {
            background-color: #e0e0e0 !important;
            color: #aaa !important;
            cursor: not-allowed;
            opacity: 0.7;
        }
        .back-button {
            background: #e9eaec;
            color: #8a8a8e;
            width: 44px;
            height: 44px;
            border-radius: 50%;
            font-size: 1.1em;
            padding: 0;
            border: none;
        }
        .back-button:hover { background: #dcdfe3; }

        .card-button {
            background-color: var(--primary-color);
            color: white;
            padding: 18px;
            width: 100%;
            border-radius: 12px;
            font-size: 1.1em;
            margin-top: 10px;
        }
        .card-button.primary { background-color: var(--primary-color); }
        .card-button:hover { filter: brightness(1.1); transform: scale(1.02); } /* 호버 효과 */

        /* --- 3. [1] 시작 화면 (라운지) --- */
        #start-screen {
            padding: 0;
            justify-content: center;
            /* [수정] 이미지 대신 그래픽 배경 */
            background: linear-gradient(180deg, var(--gradient-start) 0%, var(--gradient-end) 100%);
        }
        .start-overlay { /* 어둡게 처리 제거 -> 삭제 */
           display: none;
        }
        .start-content {
            position: relative;
            z-index: 2; /* 오버레이보다 위에 있도록 */
            text-shadow: 0 2px 5px rgba(0,0,0,0.7);
            width: 100%;
            padding: 30px;
            box-sizing: border-box;
            display: flex;
            flex-direction: column;
            justify-content: flex-end; /* 컨텐츠를 아래로 */
            flex-grow: 1; /* 화면 꽉 채우기 */
        }
        .start-content p {
            color: #eee;
            font-size: 1.2em;
            margin-bottom: 20px;
        }
        .glass-button {
            background: rgba(255, 255, 255, 0.1); /* 더 어두운 배경 */
            border: 1px solid rgba(255, 255, 255, 0.2);
            color: white;
            padding: 18px 30px;
            backdrop-filter: blur(10px);
            font-size: 1.1em;
            margin: 5px 0;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .glass-button i { margin-right: 10px; }
        .glass-button.secondary {
            background: rgba(0, 0, 0, 0.1);
             border: 1px solid rgba(255, 255, 255, 0.1);
            font-size: 1em;
        }
        .glass-button:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-2px); /* 살짝 위로 */
        }

        /* --- 4. [2] 비행 설정 화면 (카드 + Select) --- */
        .card {
            background: var(--card-bg);
            border-radius: 16px;
            width: 100%;
            padding: 20px;
            box-sizing: border-box;
            margin-bottom: 10px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }
        .card-header {
            font-size: 0.9em;
            font-weight: 700;
            color: var(--subtext-color);
            text-align: left;
            margin-bottom: 5px;
        }
        .card-arrow {
            font-size: 1.5em;
            color: #ccc;
            margin: 10px 0;
        }
        .location-select {
            width: 100%;
            padding: 15px;
            border-radius: 10px;
            border: 1px solid var(--border-color);
            font-size: 1.1em;
            background-color: var(--card-bg);
            color: var(--text-color);
            -webkit-appearance: none; /* 기본 스타일 제거 */
            -moz-appearance: none;
            appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='16' height='16' fill='%238a8a8e' viewBox='0 0 16 16'%3E%3Cpath fill-rule='evenodd' d='M1.646 4.646a.5.5 0 0 1 .708 0L8 10.293l5.646-5.647a.5.5 0 0 1 .708.708l-6 6a.5.5 0 0 1-.708 0l-6-6a.5.5 0 0 1 0-.708z'/%3E%3C/svg%3E"); /* 화살표 아이콘 */
            background-repeat: no-repeat;
            background-position: right 15px center;
            background-size: 16px;
            cursor: pointer;
        }
        .info-card {
            background-color: #e9eaec;
            text-align: center;
            padding: 15px;
            font-weight: 500;
            color: var(--subtext-color);
        }
        .info-card span {
            font-weight: 700;
            color: var(--primary-color);
            font-variant-numeric: tabular-nums;
        }

        /* --- 5. 지도 화면 (출발/도착) --- */
        .map-screen { /* 출발/도착 지도 화면 공통 */
            padding: 0;
            background-color: #aadaff;
        }
        .map-header {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            background: linear-gradient(to bottom, rgba(0,0,0,0.5), transparent);
            z-index: 20;
            padding: 20px;
            box-sizing: border-box;
            display: flex;
            align-items: center;
        }
        .map-header h2 {
            color: white;
            text-shadow: 0 1px 3px rgba(0,0,0,0.5);
            margin: 0;
            flex-grow: 1;
        }
        .map-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
        }
        .map-container img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            opacity: 1;
        }
        @keyframes pulse {
            0% { transform: translate(-50%, -50%) scale(1); }
            50% { transform: translate(-50%, -50%) scale(1.1); box-shadow: 0 6px 12px rgba(0,0,0,0.5); }
            100% { transform: translate(-50%, -50%) scale(1); }
        }
        .map-pin {
            position: absolute;
            background-color: var(--red-color);
            color: white;
            border: 3px solid white;
            border-radius: 50%;
            width: 20px;
            height: 20px;
            cursor: pointer;
            padding: 0;
            margin: 0;
            box-shadow: 0 4px 8px rgba(0,0,0,0.4);
            transform: translate(-50%, -50%);
            z-index: 10;
            animation: pulse 1.5s infinite ease-in-out;
        }
        .map-pin span {
            position: absolute;
            bottom: -25px;
            left: 50%;
            transform: translateX(-50%);
            background: white;
            color: #333;
            padding: 3px 8px;
            border-radius: 5px;
            font-size: 0.9em;
            font-weight: 700;
            white-space: nowrap;
            box-shadow: 0 1px 3px rgba(0,0,0,0.2);
        }
        .map-pin:hover {
            animation-play-state: paused;
            transform: translate(-50%, -50%) scale(1.3);
        }


        /* --- 6. [개선] 좌석 선택 화면 (그래픽) --- */
        .seat-map-container {
            width: 100%;
            flex-grow: 1; 
            background: var(--card-bg); /* 흰 배경 */
            border-radius: 30px 30px 0 0;
            box-shadow: 0 -5px 20px rgba(0,0,0,0.05);
            overflow-y: auto;
            padding: 30px 20px; /* 패딩 증가 */
            margin-top: 20px;
            box-sizing: border-box;
             /* 비행기 동체 모양 힌트 */
            border-left: 5px solid #bdc3c7; 
            border-right: 5px solid #bdc3c7;
        }
        .plane-body {
            /* 배경 제거 */
            padding-bottom: 30px; 
        }
        .cabin-label {
            font-size: 1em; /* 크기 증가 */
            font-weight: 700;
            color: var(--subtext-color);
            margin: 25px 0 10px;
            border: none; /* 구분선 제거 */
            text-align: center; 
            padding-bottom: 5px;
            position: relative;
        }
         /* 구분선 그래픽 */
        .cabin-label::before, .cabin-label::after {
            content: '';
            position: absolute;
            bottom: 0;
            height: 1px;
            width: 35%;
            background-color: #ccc;
        }
        .cabin-label::before { left: 0; }
        .cabin-label::after { right: 0; }

        .seat-row {
            display: flex;
            justify-content: center;
            align-items: center; 
            margin: 10px 0;
        }
        .seat, .aisle {
            width: 40px;
            height: 40px;
            margin: 4px; /* 간격 증가 */
            border-radius: 8px; /* 더 둥글게 */
        }
        .seat {
            background-color: #e9eaec; /* 좌석 색 */
            color: #3c3c43; /* 좌석 글씨 */
            font-size: 0.8em;
            font-weight: 700;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: background-color 0.2s, color 0.2s, transform 0.1s;
             border: 1px solid #d1d5db; /* 좌석 테두리 */
        }
        .seat:hover {
            background-color: var(--primary-color);
            color: white;
            transform: scale(1.1);
        }
        .aisle {
            width: 30px; /* 복도 너비 */
        }

         /* --- 7. [신규] 탑승권 화면 --- */
        #boarding-pass-screen { } 
        .ticket-ui {
            background: var(--card-bg);
            border-radius: 16px;
            width: 100%;
            max-width: 400px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            margin-top: 20px;
            overflow: hidden; 
            position: relative;
        }
        .ticket-header {
             background: linear-gradient(to right, #0A2C4E, #1e4a7a); 
            color: white;
            padding: 20px;
            font-size: 1.3em;
            font-weight: 700;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
         .ticket-header i { font-size: 1.5em; }
        .ticket-body {
            padding: 25px;
            display: grid;
            grid-template-columns: 1fr 1fr; 
            gap: 20px;
            text-align: left;
        }
        .ticket-section span { display: block; }
        .ticket-section .label { font-size: 0.9em; color: var(--subtext-color); margin-bottom: 5px; }
        .ticket-section .value { font-size: 1.4em; font-weight: 700; color: var(--text-color); word-break: keep-all; } 
        .ticket-time { grid-column: span 2; } 
        .ticket-barcode {
            grid-column: span 2;
            width: 80%;
            height: 50px;
            background: repeating-linear-gradient(
                to right,
                #333,
                #333 4px,
                transparent 4px,
                transparent 8px
            );
            margin: 10px auto;
        }
        .ticket-boarding-button {
            margin: 25px; /* 버튼 주변 여백 */
        }
        .ticket-ui::before {
            content: '';
            position: absolute;
            left: 20px;
            right: 20px;
            top: 75px; 
            border-top: 2px dashed #ccc;
        }

         /* --- 8. [신규] 탑승 애니메이션 화면 --- */
        #boarding-animation-screen {
            background-color: var(--bg-color); 
            justify-content: center;
            color: var(--text-color);
        }
        .boarding-map-container {
            width: 90%;
            max-width: 400px;
            height: 300px;
            border-radius: 16px;
            overflow: hidden;
            position: relative;
            background-color: #aadaff; /* 지도 배경색 */
            margin-bottom: 20px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
             /* 배경 이미지 추가 */
            background-size: cover;
            background-position: center;
        }
        #boarding-plane-icon {
            font-size: 1.8em;
            color: var(--primary-color);
            position: absolute;
            left: 10%; 
            top: 70%;
            transform: translate(-50%, -50%);
            text-shadow: 0 1px 3px rgba(0,0,0,0.3);
            /* JS가 애니메이션 설정 */
            transition: left 2.5s ease-in-out, top 2.5s ease-in-out;
        }
        #boarding-text {
            font-size: 1.5em;
            font-weight: 700;
             opacity: 0;
            animation: fadeInText 1s 0.5s forwards; 
        }
         @keyframes fadeInText { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }


        /* --- 9. 비행 화면 --- */
        #flight-screen {
            padding: 0;
            background-color: #1a1a2e;
            position: relative; 
            justify-content: center;
        }
        .flight-interior {
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            background-size: cover;
            background-position: center; 
            filter: brightness(0.7); 
            display: flex;
            justify-content: center;
            align-items: center; 
            overflow: hidden;
            z-index: 0;
        }
        .window-frame {
            width: 80%; 
            max-width: 400px;
            height: 60vh;
            background-color: #000;
            border-radius: 60px; 
            box-shadow: 0 0 50px rgba(0, 0, 0, 0.7) inset, 0 0 10px rgba(0, 0, 0, 0.5), 0 0 0 5px rgba(0,0,0,0.1) inset;
            position: relative;
            z-index: 1; 
            border: 20px solid #d1d9e0; /* 비행기 내부 플라스틱 색 */
            overflow: hidden; 
        }
        .window-view {
            width: 100%;
            height: 100%;
            background-size: cover; 
            background-position: center;
        }

        .timer-display {
            position: absolute;
            bottom: 0;
            width: 100%;
            z-index: 2; 
            background: linear-gradient(to top, rgba(0,0,0,0.9), transparent); 
            padding: 20px 20px 40px 20px;
            box-sizing: border-box;
        }
        .route-info {
            color: #F0B429;
            font-size: 1.2em;
            font-weight: 700;
            margin: 0;
        }
        .timer-label {
            color: #aaa;
            font-size: 1em;
            margin: 5px 0 0;
        }
        #timer-text {
            font-size: 3.5em; 
            color: #e0e0e0;
            font-weight: 300;
            margin: 0 0 10px 0;
            font-variant-numeric: tabular-nums;
        }
        .audio-hint {
            font-size: 0.9em;
            color: #888;
            margin: 5px 0 0;
            display: none; /* 평소엔 숨김 */
        }
        .land-button {
            background-color: var(--red-color);
            color: white;
            margin-top: 15px;
            padding: 15px 25px;
            border-radius: 25px; /* 알약 형태 */
            font-size: 1em;
        }
        .land-button:hover {
            background-color: #c82333;
        }

        /* 백색소음 조절기 */
        #sound-controls {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 15px;
        }
        .sound-btn {
            background: rgba(255, 255, 255, 0.1);
            color: #888;
            width: 44px;
            height: 44px;
            border-radius: 50%;
            font-size: 1em;
            padding: 0;
        }
        .sound-btn.active {
            background: var(--primary-color);
            color: white;
        }
        .sound-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        /* [개선] 미니맵 스타일 (선 경로) */
        #minimap {
            position: absolute;
            top: 20px; /* 상단으로 이동 */
            right: 20px;
            width: 120px; 
            height: auto; 
            background: rgba(0, 0, 0, 0.5); /* 반투명 배경 */
            backdrop-filter: blur(5px);
            border-radius: 8px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            display: flex;
            align-items: center;
            padding: 8px 10px;
            z-index: 5;
            font-size: 0.8em;
            color: #ccc;
            font-weight: bold;
        }
        #minimap span {
            width: 30px; 
            overflow: hidden;
            white-space: nowrap;
            text-align: center;
        }
        #minimap-route {
            flex-grow: 1;
            height: 4px; /* 경로 선 두께 */
            background: #555; /* 경로 선 색 */
            border-radius: 2px;
            position: relative;
            margin: 0 5px;
        }
        #minimap-plane {
            position: absolute;
            top: 50%;
            left: 0%; /* JS가 위치 조절 */
            transform: translate(-50%, -50%);
            color: var(--primary-color);
            font-size: 1.5em; /* 비행기 아이콘 크기 */
            transition: left 1s linear; /* 부드럽게 이동 */
            text-shadow: 0 0 3px black; /* 그림자 추가 */
        }

        .flight-back { /* 비행 중 뒤로가기 버튼 */
            position: absolute;
            top: 20px;
            left: 20px;
            z-index: 30;
            background: rgba(0,0,0,0.3);
            color: white;
        }


        /* --- 10. [7] 결과 화면 --- */
        #result-screen {
            justify-content: flex-start;
        }
        #result-screen h1 {
            color: var(--text-color);
            font-size: 2.5em;
            margin-top: 40px;
        }
        #result-ticket-container {
            width: 100%;
            padding: 0 20px; /* 결과 티켓 좌우 여백 */
            box-sizing: border-box;
        }

        /* --- 11. [8] 로그 화면 --- */
        #log-list-container {
            width: 100%;
        }
        #log-list-container p {
            color: var(--subtext-color);
            margin-top: 30px;
        }
        .list-header {
            width: 100%;
            margin-top: 20px;
            text-align: left;
            color: var(--subtext-color);
            font-weight: 700;
        }
        .summary-card {
            background: #0A2C4E;
        }
        .summary-card .card-header { color: #aaa; }
        #log-summary-container {
            font-size: 1.8em;
            font-weight: 700;
            color: #fff;
            text-align: left;
        }
        .chart-card {
            padding-bottom: 10px;
        }
        #chart-area {
            display: flex;
            justify-content: space-around;
            align-items: flex-end;
            height: 100px;
            border-bottom: 2px solid var(--border-color);
            padding: 0 5px;
        }
        .chart-bar {
            background-color: #0A2C4E;
            width: 10%;
            border-radius: 3px 3px 0 0;
            animation: grow 1s ease-out;
            position: relative;
        }
        .chart-bar span { /* 시간 툴팁 */
            display: none;
            position: absolute;
            top: -30px;
            left: 50%;
            transform: translateX(-50%);
            background: #333;
            color: white;
            padding: 3px 6px;
            border-radius: 3px;
            font-size: 0.8em;
            white-space: nowrap;
        }
        .chart-bar:hover span {
            display: block;
        }
        @keyframes grow {
            from { height: 0; }
        }

        /* 티켓 스타일 (결과, 로그 공통) */
        .log-ticket {
            background: var(--card-bg);
            border-left: 5px solid var(--primary-color);
            border-radius: 10px;
            padding: 15px;
            margin: 10px 0;
            width: 100%;
            box-sizing: border-box;
            text-align: left;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }
        .ticket-route {
            font-size: 1.1em;
            font-weight: 700;
            color: var(--text-color);
        }
        .ticket-route .fas {
            margin: 0 8px;
            color: var(--primary-color);
        }
        .ticket-duration, .ticket-date {
            font-size: 0.9em;
            color: var(--subtext-color);
            margin-top: 8px;
        }
        .ticket-duration strong {
            color: var(--red-color);
        }

        /* --- [신규] 집중력 체크 모달 --- */
        .modal-overlay {
            display: none; /* 평소엔 숨김 */
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.6);
            backdrop-filter: blur(5px);
            justify-content: center;
            align-items: center;
            z-index: 100;
            animation: fadeIn 0.3s;
        }
        .modal-content {
            background: var(--card-bg);
            border-radius: 20px;
            padding: 30px;
            width: 80%;
            max-width: 300px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        .modal-content h2 {
            margin-top: 0;
        }
        .modal-content p {
            margin-bottom: 25px;
            color: var(--subtext-color);
        }

        /* --- [신규] 기본 타이머 & 투두리스트 --- */
        .segment-control {
            display: flex;
            background: #e9eaec;
            border-radius: 10px;
            padding: 4px;
            margin: 20px 0;
        }
        .segment-btn {
            flex: 1;
            padding: 10px;
            border-radius: 8px;
            background: transparent;
            color: var(--subtext-color);
            font-size: 1em;
            font-weight: 700;
        }
        .segment-btn.active {
            background: var(--card-bg);
            color: var(--primary-color);
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        .timer-container {
            display: none; /* 탭에 따라 보임 */
            flex-direction: column;
            align-items: center;
            width: 100%;
        }
        .timer-container.active {
            display: flex;
        }
        .simple-timer-display {
            font-size: 5em; 
            font-weight: 700;
            color: var(--text-color);
            margin: 30px 0;
            font-variant-numeric: tabular-nums; /* 숫자 너비 고정 */
        }
        .simple-controls {
            display: flex;
            gap: 20px;
        }
        .control-btn {
            background: var(--primary-color);
            color: white;
            width: 70px;
            height: 70px;
            border-radius: 50%;
            font-size: 1.5em;
            padding: 0;
        }
        .control-btn.secondary {
            background: #e9eaec;
            color: var(--subtext-color);
        }

        /* [수정] 뽀모도로 시간 설정 (H:M:S) */
        .pomo-settings {
            display: flex;
            flex-direction: column; /* 세로 배치 */
            gap: 15px;
            width: 100%;
            margin: 10px 0 20px 0;
            padding: 20px;
            box-sizing: border-box;
            background: var(--card-bg); /* 카드 배경 */
            border-radius: 16px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }
        .setting-group {
            display: flex;
            flex-direction: column;
            align-items: flex-start; /* 왼쪽 정렬 */
        }
        .pomo-settings label {
            font-size: 0.9em;
            color: var(--subtext-color);
            margin-bottom: 8px;
            font-weight: 700;
        }
        .time-input-group {
            display: flex;
            align-items: center;
            gap: 5px;
            font-size: 1.1em;
            font-weight: 700;
            color: var(--subtext-color);
        }
        .time-input-group input[type=number] {
            width: 60px;
            padding: 10px;
            border-radius: 8px;
            border: 1px solid var(--border-color);
            font-size: 1.1em;
            text-align: center;
            font-weight: 700;
            color: var(--text-color);
        }
        /* 입력 필드 화살표 숨기기 */
        .time-input-group input::-webkit-outer-spin-button,
        .time-input-group input::-webkit-inner-spin-button {
          -webkit-appearance: none;
          margin: 0;
        }
        .time-input-group input[type=number] {
          -moz-appearance: textfield;
        }

        /* 투두리스트 */
        #todo-form {
            display: flex;
            width: 100%;
        }
        #todo-input {
            flex-grow: 1;
            border: none;
            background: #e9eaec; /* 입력창 배경 */
            border-radius: 10px;
            padding: 15px;
            font-size: 1em;
        }
        #todo-input:focus {
            outline: none;
            box-shadow: 0 0 0 2px var(--primary-color);
        }
        .todo-add-btn {
            background: var(--primary-color);
            color: white;
            width: 50px;
            height: 50px;
            border-radius: 10px;
            font-size: 1.2em;
            padding: 0;
            margin-left: 10px;
        }
        #todo-list-container {
            list-style: none;
            padding: 0;
            width: 100%;
            margin-top: 10px;
        }
        .todo-item {
            background: var(--card-bg);
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            font-size: 1.1em;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            cursor: pointer;
        }
        .todo-item.done {
            text-decoration: line-through;
            color: #aaa;
            opacity: 0.7;
        }
        .todo-item i.fa-circle, .todo-item i.fa-check-circle {
            color: var(--primary-color);
            margin-right: 15px;
            min-width: 20px; /* 아이콘 공간 확보 */
            text-align: center;
        }
        .todo-item.done i.fa-check-circle {
            color: #aaa;
        }
        .todo-item span {
             flex-grow: 1; /* 텍스트 영역 확장 */
             text-align: left;
             word-break: break-all; /* 긴 텍스트 줄바꿈 */
        }
        .todo-item .delete-btn {
            margin-left: auto;
            background: none;
            color: var(--red-color);
            font-size: 1.1em;
            padding: 5px;
            width: auto; /* 크기 자동 */
            height: auto;
        }
        .todo-item .delete-btn:hover {
            color: darkred;
        }

    </style>
    </head>
<body>
    <audio id="sound-inflight" loop preload="auto">
        <source src="https://archive.org/download/AirplaneInterior/AirplaneInterior.mp3" type="audio/mpeg">
    </audio>
    <audio id="sound-rain" loop preload="auto">
        <source src="https://archive.org/download/RainOnTheWindow/RainOnTheWindow.mp3" type="audio/mpeg">
    </audio>
    <audio id="sound-landing" preload="auto">
        <source src="https://archive.org/download/AirplaneLanding-SoundBible.com-2035388002/AirplaneLanding-SoundBible.com-2035388002.mp3" type="audio/mpeg">
    </audio>
    <audio id="sound-chime" preload="auto">
        <source src="https://archive.org/download/StoreDoorChime/StoreDoorChime.mp3" type="audio/mpeg">
    </audio>


    <div class="app-container">
        
        <div id="start-screen" class="screen active">
            <div class="start-background"></div> 
            <div class="start-content">
                <h1>Flight Study</h1>
                <p>당신의 공부 비행, 지금 이륙합니다.</p>
                <button id="start-button" class="glass-button">
                    <i class="fas fa-plane-departure"></i> 비행 시작하기
                </button>
                <button id="simple-study-button" class="glass-button secondary">
                    <i class="fas fa-stopwatch"></i> 기본 타이머
                </button>
                <button id="todo-button" class="glass-button secondary">
                    <i class="fas fa-tasks"></i> 스터디 체크리스트
                </button>
                <button id="view-log-button" class="glass-button secondary">
                    <i class="fas fa-history"></i> 나의 비행 기록
                </button>
            </div>
        </div>

        <div id="flight-config-screen" class="screen">
            <div class="header">
                <button class="back-button" data-target="start"><i class="fas fa-arrow-left"></i></button>
                <h2>비행 설정</h2>
            </div>
            <div class="content-area">
                <div class="card">
                    <div class="card-header">출발 공항</div>
                    <select id="departure-select" class="location-select">
                        <option value="" disabled selected>공항 선택</option>
                        <option value="ICN">인천 (ICN)</option>
                        <option value="GMP">김포 (GMP)</option>
                        <option value="CJU">청주 (CJU)</option>
                        <option value="TAE">대구 (TAE)</option>
                        <option value="PUS">김해 (PUS)</option>
                    </select>
                </div>
                <i class="fas fa-arrow-down card-arrow"></i>
                <div class="card">
                    <div class="card-header">도착 도시</div>
                    <select id="destination-select" class="location-select">
                         <option value="" disabled selected>도시 선택</option>
                        <option value="Tokyo">도쿄 (NRT)</option>
                        <option value="Fukushima">후쿠시마 (FKS)</option>
                        <option value="Beijing">베이징 (PEK)</option>
                        <option value="Shanghai">상하이 (PVG)</option>
                        <option value="Ulaanbaatar">울란바토르 (UBN)</option>
                    </select>
                </div>
                <div id="auto-flight-time-display" class="card info-card">
                    예상 비행 시간: <span>--:--:--</span>
                </div>
                <button id="go-to-boarding-pass-button" class="card-button primary" disabled>탑승권 확인</button> 
            </div>
        </div>

        <div id="boarding-pass-screen" class="screen">
             <div class="header">
                 <button class="back-button" data-target="start"><i class="fas fa-arrow-left"></i></button>
                <h2>탑승권 정보</h2>
            </div>
            <div class="content-area column">
                <div class="ticket-ui">
                    <div class="ticket-header">
                        <span>FLIGHT STUDY</span>
                        <i class="fas fa-plane"></i>
                    </div>
                    <div class="ticket-body">
                         <div class="ticket-section">
                            <span class="label">FROM</span>
                            <span id="bp-departure" class="value"></span>
                        </div>
                        <div class="ticket-section">
                            <span class="label">TO</span>
                            <span id="bp-destination" class="value"></span>
                        </div>
                        <div class="ticket-section ticket-time">
                            <span class="label">ESTIMATED FLIGHT TIME</span>
                            <span id="bp-flight-time" class="value"></span>
                        </div>
                        <div class="ticket-barcode"></div>
                    </div>
                </div>
                 <button id="board-plane-button" class="card-button primary ticket-boarding-button">탑승하기</button>
            </div>
        </div>
        
        <div id="boarding-animation-screen" class="screen">
             <div class="boarding-map-container" style="background-image: url('https://i.imgur.com/a4gT42G.png'); background-size: cover; background-position: center;">
                 <div id="boarding-plane-icon">✈️</div>
             </div>
            <p id="boarding-text">탑승 중...</p>
        </div>

        <div id="seat-screen" class="screen">
            <div class="header">
                 <button class="back-button" data-target="start"><i class="fas fa-arrow-left"></i></button>
                <h2>좌석 선택</h2>
            </div>
            <p class="subtitle">어느 좌석에서 공부하시겠어요?</p>
            <div class="seat-map-container">
                <div class="plane-body">
                    <div class="cabin-label">FIRST</div>
                    <div class="seat-row">
                        <div class="seat" data-seat="1A">1A</div>
                        <div class="seat" data-seat="1B">1B</div>
                        <div class="aisle"></div>
                        <div class="seat" data-seat="1C">1C</div>
                        <div class="seat" data-seat="1D">1D</div>
                    </div>
                    <div class="cabin-label">ECONOMY</div>
                    <div class="seat-row">
                        <div class="seat" data-seat="20A">20A</div>
                        <div class="seat" data-seat="20B">20B</div>
                        <div class="seat" data-seat="20C">20C</div>
                        <div class="aisle"></div>
                        <div class="seat" data-seat="20D">20D</div>
                        <div class="seat" data-seat="20E">20E</div>
                        <div class="seat" data-seat="20F">20F</div>
                    </div>
                     <div class="seat-row">
                        <div class="seat" data-seat="21A">21A</div>
                        <div class="seat" data-seat="21B">21B</div>
                        <div class="seat" data-seat="21C">21C</div>
                        <div class="aisle"></div>
                        <div class="seat" data-seat="21D">21D</div>
                        <div class="seat" data-seat="21E">21E</div>
                        <div class="seat" data-seat="21F">21F</div>
                    </div>
                </div>
            </div>
        </div>

        <div id="flight-screen" class="screen">
            <div class="flight-interior" style="background-image: url('https://i.imgur.com/kSjJmE6.jpeg');">
                <div class="window-frame">
                    <div class="window-view" style="background-image: url('https://i.imgur.com/I2ZmE1w.gif');"></div>
                </div>
            </div>
            <div class="timer-display">
                <div id="sound-controls">
                    <button class="sound-btn active" data-sound="inflight"><i class="fas fa-plane"></i></button>
                    <button class="sound-btn" data-sound="rain"><i class="fas fa-cloud-showers-heavy"></i></button>
                    <button class="sound-btn" data-sound="mute"><i class="fas fa-volume-mute"></i></button>
                </div>
                
                <p id="route-display" class="route-info"></p>
                <p class="timer-label">남은 비행 시간</p>
                <h3 id="timer-text">00:00:00</h3>
                <p id="audio-hint" class="audio-hint">(소리가 안 들리면 화면을 클릭하세요)</p>
                <button id="land-button" class="land-button">내리기</button>
            </div>
            
            <div id="minimap">
                <span id="minimap-departure">DEP</span>
                <div id="minimap-route">
                     <div id="minimap-plane">✈️</div> </div>
                 <span id="minimap-destination">ARR</span>
            </div>
            
            <button class="back-button flight-back" data-target="start"><i class="fas fa-arrow-left"></i></button>
        </div>

        <div id="result-screen" class="screen">
            <div class="header"></div>
            <div class="content-area column">
                <h1><i class="fas fa-plane-arrival"></i> 착륙</h1>
                <p class="subtitle">공부 비행을 완료했습니다! 티켓을 확인하세요.</p>
                <div id="result-ticket-container">
                    </div>
                <button id="go-home-button" class="card-button primary">홈으로 돌아가기</button>
            </div>
        </div>

        <div id="log-screen" class="screen">
            <div class="header">
                <button class="back-button" data-target="start"><i class="fas fa-arrow-left"></i></button>
                <h2>나의 비행 기록</h2>
            </div>
            <div class="content-area scrollable">
                <div class="card summary-card">
                    <div class="card-header">총 공부 시간</div>
                    <div id="log-summary-container">0초</div>
                </div>
                <div class="card chart-card">
                    <div class="card-header">최근 10회 기록 (분)</div>
                    <div id="chart-area"></div>
                </div>
                <div class="card-header list-header">모든 기록</div>
                <div id="log-list-container">
                    <p>아직 기록이 없습니다.</p>
                </div>
            </div>
        </div>
        
        <div id="simple-study-screen" class="screen">
            <div class="header">
                 <button class="back-button" data-target="start"><i class="fas fa-arrow-left"></i></button>
                <h2>기본 타이머</h2>
            </div>
            <div class="content-area">
                <div class="segment-control">
                    <button id="pomo-tab" class="segment-btn active" data-target="pomo-timer">뽀모도로</button>
                    <button id="stopwatch-tab" class="segment-btn" data-target="stopwatch-timer">스톱워치</button>
                </div>
                
                <div id="pomo-timer" class="timer-container active">
                    <div class="pomo-settings card">
                         <div class="setting-group">
                            <label>공부 시간 (최대 24시간)</label>
                            <div class="time-input-group">
                                <input type="number" id="pomo-study-hours" min="0" max="24" value="0"> H :
                                <input type="number" id="pomo-study-minutes" min="0" max="59" value="25"> M :
                                <input type="number" id="pomo-study-seconds" min="0" max="59" value="0"> S
                            </div>
                        </div>
                         <div class="setting-group">
                             <label>휴식 시간 (최대 24시간)</label>
                             <div class="time-input-group">
                                <input type="number" id="pomo-break-hours" min="0" max="24" value="0"> H :
                                <input type="number" id="pomo-break-minutes" min="0" max="59" value="5"> M :
                                <input type="number" id="pomo-break-seconds" min="0" max="59" value="0"> S
                            </div>
                        </div>
                    </div>
                    <p id="pomo-status" class="timer-label">공부할 시간!</p>
                    <div id="pomo-timer-display" class="simple-timer-display">00:25:00</div>
                    <div class="simple-controls">
                        <button id="pomo-start-pause" class="control-btn"><i class="fas fa-play"></i></button>
                        <button id="pomo-reset" class="control-btn secondary"><i class="fas fa-undo"></i></button>
                    </div>
                </div>
                
                <div id="stopwatch-timer" class="timer-container">
                    <p class="timer-label">공부 시간 측정</p>
                    <div id="stopwatch-timer-display" class="simple-timer-display">00:00:00</div>
                    <div class="simple-controls">
                        <button id="stopwatch-start-pause" class="control-btn"><i class="fas fa-play"></i></button>
                        <button id="stopwatch-reset" class="control-btn secondary"><i class="fas fa-undo"></i></button>
                    </div>
                </div>
            </div>
        </div>

        <div id="checklist-screen" class="screen">
            <div class="header">
                 <button class="back-button" data-target="start"><i class="fas fa-arrow-left"></i></button>
                <h2>스터디 체크리스트</h2>
            </div>
            <div class="content-area scrollable">
                <div class="card">
                    <form id="todo-form">
                        <input type="text" id="todo-input" placeholder="오늘 할 일을 추가하세요..." autocomplete="off">
                        <button type="submit" class="todo-add-btn"><i class="fas fa-plus"></i></button>
                    </form>
                </div>
                <ul id="todo-list-container">
                    </ul>
            </div>
        </div>

    </div> <div id="focus-check-modal" class="modal-overlay">
        <div class="modal-content">
            <h2><i class="fas fa-coffee"></i></h2>
            <h2>잠깐!</h2>
            <p>계속 공부하고 계신가요?</p>
            <button id="focus-continue-button" class="card-button primary">네, 계속할게요!</button>
        </div>
    </div>
    
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // --- 1. 요소 선택 ---
            const screens = {
                start: document.getElementById('start-screen'),
                flightConfig: document.getElementById('flight-config-screen'), 
                boardingPass: document.getElementById('boarding-pass-screen'), 
                boardingAnimation: document.getElementById('boarding-animation-screen'), 
                seat: document.getElementById('seat-screen'), 
                flight: document.getElementById('flight-screen'),
                result: document.getElementById('result-screen'), 
                log: document.getElementById('log-screen'),       
                simpleStudy: document.getElementById('simple-study-screen'),
                checklist: document.getElementById('checklist-screen'), 
            };
            
            const buttons = {
                start: document.getElementById('start-button'),
                viewLog: document.getElementById('view-log-button'), 
                simpleStudy: document.getElementById('simple-study-button'), 
                todo: document.getElementById('todo-button'), 
                goToBoardingPass: document.getElementById('go-to-boarding-pass-button'), // [수정] 버튼 ID 변경
                boardPlane: document.getElementById('board-plane-button'), 
                land: document.getElementById('land-button'),
                goHome: document.getElementById('go-home-button'),
                focusContinue: document.getElementById('focus-continue-button'), 
                pomoTab: document.getElementById('pomo-tab'),
                stopwatchTab: document.getElementById('stopwatch-tab'), 
                pomoStartPause: document.getElementById('pomo-start-pause'),
                pomoReset: document.getElementById('pomo-reset'),
                stopwatchStartPause: document.getElementById('stopwatch-start-pause'), 
                stopwatchReset: document.getElementById('stopwatch-reset'),         
            };
            
            const textElements = {
                routeDisplay: document.getElementById('route-display'),
                timer: document.getElementById('timer-text'),
                audioHint: document.getElementById('audio-hint'), 
                pomoStatus: document.getElementById('pomo-status'),
                pomoTimer: document.getElementById('pomo-timer-display'),
                stopwatchTimer: document.getElementById('stopwatch-timer-display'), 
                autoFlightTime: document.querySelector('#auto-flight-time-display span'), 
                bpDeparture: document.getElementById('bp-departure'),
                bpDestination: document.getElementById('bp-destination'),
                bpFlightTime: document.getElementById('bp-flight-time'),
            };
            
            const containers = {
                resultTicket: document.getElementById('result-ticket-container'), 
                logSummary: document.getElementById('log-summary-container'), 
                logChart: document.getElementById('chart-area'),            
                logList: document.getElementById('log-list-container'),
                pomoTimer: document.getElementById('pomo-timer'),
                stopwatchTimer: document.getElementById('stopwatch-timer'), 
                todoList: document.getElementById('todo-list-container'), 
                boardingMap: document.querySelector('.boarding-map-container'), 
            };
            
            const inputs = {
                departureSelect: document.getElementById('departure-select'), 
                destinationSelect: document.getElementById('destination-select'), 
                pomoStudyHours: document.getElementById('pomo-study-hours'),
                pomoStudyMinutes: document.getElementById('pomo-study-minutes'),
                pomoStudySeconds: document.getElementById('pomo-study-seconds'),
                pomoBreakHours: document.getElementById('pomo-break-hours'),
                pomoBreakMinutes: document.getElementById('pomo-break-minutes'),
                pomoBreakSeconds: document.getElementById('pomo-break-seconds'),
                todoForm: document.getElementById('todo-form'),
                todoInput: document.getElementById('todo-input'),
            };
            
            const mapElements = { 
                minimap: document.getElementById('minimap'),
                plane: document.getElementById('minimap-plane'),
                departure: document.getElementById('minimap-departure'),
                destination: document.getElementById('minimap-destination'),
                boardingPlaneIcon: document.getElementById('boarding-plane-icon'), 
            };
            
            const seatMapButtons = document.querySelectorAll('.seat');
            const soundControlButtons = document.querySelectorAll('.sound-btn');
            const focusModal = document.getElementById('focus-check-modal');
            
            const audio = {
                inflight: document.getElementById('sound-inflight'),
                rain: document.getElementById('sound-rain'), 
                landing: document.getElementById('sound-landing'),
                chime: document.getElementById('sound-chime'), 
            };

            // --- 2. 데이터 및 상태 변수 ---
            
            // [복구/오류 수정] 실제 비행 시간(분) 데이터 (정확성 향상)
            const flightTimes = {
              'ICN': { 'Tokyo': 150, 'Fukushima': 160, 'Beijing': 120, 'Ulaanbaatar': 210, 'Shanghai': 110 }, 
              'GMP': { 'Tokyo': 130, 'Fukushima': 150, 'Beijing': 125, 'Ulaanbaatar': 210, 'Shanghai': 115 }, 
              'PUS': { 'Tokyo': 100, 'Fukushima': 110, 'Beijing': 140, 'Ulaanbaatar': 240, 'Shanghai': 110 }, 
              'TAE': { 'Tokyo': 110, 'Fukushima': 120, 'Beijing': 150, 'Ulaanbaatar': 235, 'Shanghai': 100 }, 
              'CJU': { 'Tokyo': 120, 'Fukushima': 130, 'Beijing': 140, 'Ulaanbaatar': 225, 'Shanghai': 90 }  
            };
            
            const locationCodes = { // 미니맵용 공항 코드
                'ICN': 'ICN', 'GMP': 'GMP', 'CJU': 'CJU', 'TAE': 'TAE', 'PUS': 'PUS',
                'Tokyo': 'NRT', 'Fukushima': 'FKS', 'Beijing': 'PEK', 'Ulaanbaatar': 'UBN', 'Shanghai': 'PVG'
            };
             const locationFullNames = { // 표시용 전체 이름
                'ICN': '인천', 'GMP': '김포', 'CJU': '청주', 'TAE': '대구', 'PUS': '김해',
                'Tokyo': '도쿄', 'Fukushima': '후쿠시마', 'Beijing': '베이징', 'Ulaanbaatar': '울란바토르', 'Shanghai': '상하이'
            };
            
            // 탑승 애니메이션용 좌표 (백분율) - 지도 이미지에 맞게 재조정 필요 시 수정
            const locationCoords = {
                'ICN': { x: 28, y: 75 }, 'GMP': { x: 30, y: 74 }, 'CJU': { x: 32, y: 65 }, 'TAE': { x: 45, y: 60 }, 'PUS': { x: 50, y: 63 },
                'Tokyo': { x: 88, y: 70 }, 'Fukushima': { x: 87, y: 60 }, 'Beijing': { x: 60, y: 45 }, 'Shanghai': { x: 70, y: 75 }, 'Ulaanbaatar': { x: 55, y: 20 }
            };

            // 비행 타이머 변수
            let selectedDepartureKey = null; 
            let selectedDestinationKey = null; 
            let currentRoute = "";
            let originalTotalSeconds = 0; 
            let totalSecondsLeft = 0;   
            let timerInterval = null;
            let focusCheckInterval = null; 
            let currentSound = audio.inflight; 

            // 기본 타이머 변수
            let pomoInterval = null;
            let pomoStudyDuration = 25 * 60; 
            let pomoBreakDuration = 5 * 60; 
            let pomoTimeLeft = pomoStudyDuration;
            let pomoIsBreak = false;
            let pomoIsRunning = false;
            
            let stopwatchInterval = null;    // [복구] 스톱워치
            let stopwatchTimeElapsed = 0;
            let stopwatchIsRunning = false;
            
            let todos = [];

            // --- 3. 핵심 기능 함수 (화면 전환) ---

            function showScreen(screenName) {
                const currentActive = document.querySelector('.screen.active');
                if (currentActive) {
                    currentActive.classList.remove('active');
                }
                setTimeout(() => {
                     if (screens[screenName]) { 
                         screens[screenName].classList.add('active');
                     } else {
                         console.error("Screen not found:", screenName);
                     }
                }, 50); 
            }

            // --- 4. 비행 스터디 기능 (Flight Study) ---
            
            // 비행 설정 화면 업데이트 (선택 값 확인 및 시간 표시)
            function updateFlightConfigScreen() {
                selectedDepartureKey = inputs.departureSelect.value;
                selectedDestinationKey = inputs.destinationSelect.value;
                
                if (selectedDepartureKey && selectedDestinationKey) {
                    const timeInMinutes = flightTimes[selectedDepartureKey][selectedDestinationKey];
                    textElements.autoFlightTime.textContent = formatFlightTimeAuto(timeInMinutes * 60);
                     buttons.goToBoardingPass.disabled = false; // [수정] 버튼 활성화
                } else {
                     textElements.autoFlightTime.textContent = '--:--:--';
                     buttons.goToBoardingPass.disabled = true; // [수정] 버튼 비활성화
                }
            }
            
            // HH:MM:SS 포맷 함수 (비행 시간 표시용)
             function formatFlightTimeAuto(totalSeconds) {
                let h = String(Math.floor(totalSeconds / 3600)).padStart(2, '0');
                let m = String(Math.floor((totalSeconds % 3600) / 60)).padStart(2, '0');
                let s = String(totalSeconds % 60).padStart(2, '0');
                return `${h}:${m}:${s}`;
            }

            // 자동 계산된 시간으로 탑승권 화면으로 이동
            function goToBoardingPass() {
                const timeInMinutes = flightTimes[selectedDepartureKey][selectedDestinationKey];
                
                if (typeof timeInMinutes !== 'number') {
                     alert('비행 시간을 계산할 수 없습니다. 노선을 다시 선택해주세요.');
                     inputs.destinationSelect.value = "";
                     selectedDestinationKey = null;
                     updateFlightConfigScreen();
                     return;
                }
                
                originalTotalSeconds = timeInMinutes * 60; 
                totalSecondsLeft = originalTotalSeconds;     
                
                // 탑승권 정보 채우기
                textElements.bpDeparture.textContent = locationFullNames[selectedDepartureKey] + ` (${locationCodes[selectedDepartureKey]})`;
                textElements.bpDestination.textContent = locationFullNames[selectedDestinationKey] + ` (${locationCodes[selectedDestinationKey]})`;
                textElements.bpFlightTime.textContent = formatFlightTimeAuto(originalTotalSeconds);

                // 미니맵용 공항 코드 설정
                mapElements.departure.textContent = locationCodes[selectedDepartureKey];
                mapElements.destination.textContent = locationCodes[selectedDestinationKey];
                
                // 루트 텍스트 설정
                currentRoute = `${locationFullNames[selectedDepartureKey]} <i class="fas fa-plane"></i> ${locationFullNames[selectedDestinationKey]}`;
                textElements.routeDisplay.innerHTML = currentRoute; 
                
                showScreen('boardingPass'); 
            }
            
             // 탑승 애니메이션 시작
            function startBoardingAnimation() {
                 showScreen('boardingAnimation');
                 
                 const startCoords = locationCoords[selectedDepartureKey];
                 const endCoords = locationCoords[selectedDestinationKey];

                 mapElements.boardingPlaneIcon.style.left = `${startCoords.x}%`;
                 mapElements.boardingPlaneIcon.style.top = `${startCoords.y}%`;
                 mapElements.boardingPlaneIcon.style.transition = 'none'; 
                 
                 setTimeout(() => {
                    mapElements.boardingPlaneIcon.style.transition = 'left 2.5s ease-in-out, top 2.5s ease-in-out';
                    mapElements.boardingPlaneIcon.style.left = `${endCoords.x}%`;
                    mapElements.boardingPlaneIcon.style.top = `${endCoords.y}%`;
                 }, 100);

                setTimeout(() => {
                    showScreen('seat');
                    mapElements.boardingPlaneIcon.style.transition = 'none'; 
                    mapElements.boardingPlaneIcon.style.left = '10%'; 
                    mapElements.boardingPlaneIcon.style.top = '70%'; 
                 }, 3000); // 3초
            }

            function startFlightTimer() {
                if (originalTotalSeconds >= 3600 && !focusCheckInterval) {
                    focusCheckInterval = setInterval(showFocusCheck, 3600 * 1000); // 1시간
                }
                mapElements.plane.style.left = '0%'; // 미니맵 초기화
                mapElements.plane.style.transition = `left 1s linear`; // 부드러운 이동
                
                timerInterval = setInterval(updateFlightDisplay, 1000); 
            }
            
            function updateFlightDisplay() {
                if (totalSecondsLeft <= 0) {
                    finishFlight(originalTotalSeconds); 
                    return;
                }
                totalSecondsLeft--; 
                
                textElements.timer.textContent = formatFlightTimeAuto(totalSecondsLeft); // HH:MM:SS 포맷 사용
                
                // 미니맵 비행기 위치 업데이트
                const progress = (originalTotalSeconds - totalSecondsLeft) / originalTotalSeconds;
                mapElements.plane.style.left = `${progress * 100}%`;
            }

            function showFocusCheck() {
                clearInterval(timerInterval);
                stopAllSounds(); 
                audio.chime.play(); 
                focusModal.style.display = 'flex'; 
            }

            function hideFocusCheck() {
                focusModal.style.display = 'none';
                if (currentSound) {
                    currentSound.play().catch(e => console.log("Focus check sound error:", e)); 
                }
                startFlightTimer(); 
            }

            function startFlight() {
                showScreen('flight');
                
                // 모든 오디오 로드 시도
                Object.values(audio).forEach(a => a.load());
                
                currentSound = audio.inflight;
                const playPromise = currentSound.play();
                if (playPromise !== undefined) {
                    playPromise.then(_=>{}).catch(error => { 
                        console.log("오디오 자동 재생 실패:", error);
                        textElements.audioHint.style.display = 'block';
                        screens.flight.onclick = () => {
                             if (currentSound) currentSound.play(); 
                            textElements.audioHint.style.display = 'none';
                            screens.flight.onclick = null; 
                        };
                    });
                }
                startFlightTimer(); // 타이머 시작
            }

            function stopAllSounds() {
                audio.inflight.pause();
                audio.rain.pause();
                audio.inflight.currentTime = 0;
                audio.rain.currentTime = 0;
            }

            function stopFlightAndSound() {
                clearInterval(timerInterval); 
                clearInterval(focusCheckInterval); 
                focusCheckInterval = null;
                
                stopAllSounds(); 
                
                mapElements.plane.style.transition = 'none';
                mapElements.plane.style.left = '0%';
                
                textElements.audioHint.style.display = 'none'; 
                if (screens.flight.onclick) {
                     screens.flight.onclick = null;
                }
            }

            function finishFlight(studiedSeconds) {
                stopFlightAndSound();
                audio.landing.play().catch(e => console.log("Landing sound error:", e)); 
                const savedLog = saveLog("Flight", currentRoute, studiedSeconds); 
                if (savedLog) renderTicket(containers.resultTicket, savedLog); 
                showScreen('result');
                resetTrip(); 
            }
            
            function landImmediately() {
                const studiedSeconds = originalTotalSeconds - totalSecondsLeft; 
                stopFlightAndSound();
                audio.landing.play().catch(e => console.log("Landing sound error:", e)); 
                const savedLog = saveLog("Flight", currentRoute, studiedSeconds); 
                if (savedLog) renderTicket(containers.resultTicket, savedLog); 
                showScreen('result');
                resetTrip(); 
            }
            
            function resetTrip() {
                inputs.departureSelect.value = "";
                inputs.destinationSelect.value = "";
                selectedDepartureKey = null;
                selectedDestinationKey = null;
                
                currentRoute = "";
                originalTotalSeconds = 0;
                totalSecondsLeft = 0;
                textElements.timer.textContent = "00:00:00";
                updateFlightConfigScreen(); 
                soundControlButtons.forEach(btn => btn.classList.remove('active'));
                soundControlButtons[0].classList.add('active'); 
                currentSound = audio.inflight;
                textElements.autoFlightTime.textContent = '--:--:--'; 
            }
            
            // --- 5. [수정] 기본 스터디 기능 (뽀모도로 H:M:S, 스톱워치 복구) ---
            
            // 뽀모도로 HH:MM:SS 포맷
            function formatPomoDisplayTime(totalSeconds) {
                 if (totalSeconds < 0) totalSeconds = 0; // 음수 방지
                let h = String(Math.floor(totalSeconds / 3600)).padStart(2, '0');
                let m = String(Math.floor((totalSeconds % 3600) / 60)).padStart(2, '0');
                let s = String(totalSeconds % 60).padStart(2, '0');
                return `${h}:${m}:${s}`;
            }

            // 스톱워치 MM:SS:ms 포맷
            function formatStopwatch(ms) {
                const totalSec = Math.floor(ms / 1000);
                const m = String(Math.floor(totalSec / 60) % 60).padStart(2, '0');
                const s = String(totalSec % 60).padStart(2, '0');
                const cs = String(Math.floor(ms / 10) % 100).padStart(2, '0');
                return `${m}:${s}:${cs}`;
            }
            
            // 뽀모도로 로직
            function startPausePomo() {
                if (pomoIsRunning) {
                    clearInterval(pomoInterval);
                    buttons.pomoStartPause.innerHTML = '<i class="fas fa-play"></i>';
                    pomoIsRunning = false;
                } else {
                     if (pomoTimeLeft <= 0) { // 리셋 상태 확인 및 시간 설정
                         pomoStudyDuration = getTimeFromPomoInputs('study');
                         pomoBreakDuration = getTimeFromPomoInputs('break');
                         if (pomoStudyDuration < 1) { 
                              alert("공부 시간은 최소 1초 이상 설정해야 합니다.");
                              return;
                         }
                          pomoTimeLeft = pomoStudyDuration;
                          textElements.pomoStatus.textContent = "공부할 시간!";
                          textElements.pomoTimer.textContent = formatPomoDisplayTime(pomoTimeLeft);
                     }
                    
                     if (pomoTimeLeft > 0) { // 시간이 설정되어 있으면 시작
                        pomoInterval = setInterval(updatePomo, 1000);
                        buttons.pomoStartPause.innerHTML = '<i class="fas fa-pause"></i>';
                        pomoIsRunning = true;
                         disablePomoInputs(true); 
                     }
                }
            }
            
            function updatePomo() {
                pomoTimeLeft--;
                textElements.pomoTimer.textContent = formatPomoDisplayTime(pomoTimeLeft);
                
                if (pomoTimeLeft < 0) {
                    clearInterval(pomoInterval);
                    audio.chime.play();
                    const lastDuration = pomoIsBreak ? pomoBreakDuration : pomoStudyDuration;
                    const logType = pomoIsBreak ? "Break" : "Pomodoro"; 
                    
                    pomoIsBreak = !pomoIsBreak;
                    
                    pomoStudyDuration = getTimeFromPomoInputs('study');
                    pomoBreakDuration = getTimeFromPomoInputs('break');
                    pomoTimeLeft = pomoIsBreak ? pomoBreakDuration : pomoStudyDuration;

                    textElements.pomoStatus.textContent = pomoIsBreak ? "휴식 시간!" : "공부할 시간!";
                    textElements.pomoTimer.textContent = formatPomoDisplayTime(pomoTimeLeft);
                    buttons.pomoStartPause.innerHTML = '<i class="fas fa-play"></i>';
                    pomoIsRunning = false;
                    disablePomoInputs(false); 
                    
                    if (!pomoIsBreak) {
                        saveLog(logType, `${formatStudyTimeMinutes(lastDuration)} 세션`, lastDuration);
                    }
                }
            }
            
            function resetPomo() {
                clearInterval(pomoInterval);
                pomoIsBreak = false;
                pomoIsRunning = false;
                pomoStudyDuration = getTimeFromPomoInputs('study');
                pomoTimeLeft = pomoStudyDuration; 
                textElements.pomoStatus.textContent = "공부할 시간!";
                textElements.pomoTimer.textContent = formatPomoDisplayTime(pomoTimeLeft);
                buttons.pomoStartPause.innerHTML = '<i class="fas fa-play"></i>';
                disablePomoInputs(false); 
            }
            
            function getTimeFromPomoInputs(type) {
                const hoursInput = inputs[`pomo${type.charAt(0).toUpperCase() + type.slice(1)}Hours`];
                const minutesInput = inputs[`pomo${type.charAt(0).toUpperCase() + type.slice(1)}Minutes`];
                const secondsInput = inputs[`pomo${type.charAt(0).toUpperCase() + type.slice(1)}Seconds`];
                
                // 입력 필드가 존재하는지 확인
                const hours = hoursInput ? parseInt(hoursInput.value) || 0 : 0;
                const minutes = minutesInput ? parseInt(minutesInput.value) || 0 : 0;
                const seconds = secondsInput ? parseInt(secondsInput.value) || 0 : 0;
                
                let totalSeconds = (hours * 3600) + (minutes * 60) + seconds;
                if (totalSeconds > 24 * 3600) totalSeconds = 24 * 3600;
                // 최소 1초 보장 (리셋 시)
                if (totalSeconds < 1 && pomoTimeLeft <= 0) totalSeconds = 1; 
                return totalSeconds;
            }
            
            function disablePomoInputs(disabled) {
                 inputs.pomoStudyHours.disabled = disabled;
                 inputs.pomoStudyMinutes.disabled = disabled;
                 inputs.pomoStudySeconds.disabled = disabled;
                 inputs.pomoBreakHours.disabled = disabled;
                 inputs.pomoBreakMinutes.disabled = disabled;
                 inputs.pomoBreakSeconds.disabled = disabled;
            }
            
            function validatePomoTimeInput(input, max) {
                if (!input) return; // 요소 없으면 종료
                input.value = input.value.replace(/[^0-9]/g, ''); 
                let value = parseInt(input.value) || 0;
                if (value < 0) value = 0;
                if (value > max) value = max;
                input.value = value; 
                // 즉시 리셋 호출하여 시간 표시 업데이트
                resetPomo(); 
            }
            // 각 H, M, S 입력 필드에 리스너 추가
            inputs.pomoStudyHours.addEventListener('input', () => validatePomoTimeInput(inputs.pomoStudyHours, 24));
            inputs.pomoStudyMinutes.addEventListener('input', () => validatePomoTimeInput(inputs.pomoStudyMinutes, 59));
            inputs.pomoStudySeconds.addEventListener('input', () => validatePomoTimeInput(inputs.pomoStudySeconds, 59));
            inputs.pomoBreakHours.addEventListener('input', () => validatePomoTimeInput(inputs.pomoBreakHours, 24));
            inputs.pomoBreakMinutes.addEventListener('input', () => validatePomoTimeInput(inputs.pomoBreakMinutes, 59));
            inputs.pomoBreakSeconds.addEventListener('input', () => validatePomoTimeInput(inputs.pomoBreakSeconds, 59));

            // [복구] 스톱워치 로직
            function startPauseStopwatch() {
                if (stopwatchIsRunning) {
                    clearInterval(stopwatchInterval);
                    buttons.stopwatchStartPause.innerHTML = '<i class="fas fa-play"></i>';
                    stopwatchIsRunning = false;
                } else {
                    const startTime = Date.now() - stopwatchTimeElapsed;
                    stopwatchInterval = setInterval(() => {
                        stopwatchTimeElapsed = Date.now() - startTime;
                        textElements.stopwatchTimer.textContent = formatStopwatch(stopwatchTimeElapsed);
                    }, 10); 
                    buttons.stopwatchStartPause.innerHTML = '<i class="fas fa-pause"></i>';
                    stopwatchIsRunning = true;
                }
            }
            
            function resetStopwatch() {
                clearInterval(stopwatchInterval);
                if (stopwatchTimeElapsed > 0) {
                    saveLog("Stopwatch", "자유 측정", Math.floor(stopwatchTimeElapsed / 1000));
                }
                stopwatchTimeElapsed = 0;
                stopwatchIsRunning = false;
                textElements.stopwatchTimer.textContent = "00:00:00";
                buttons.stopwatchStartPause.innerHTML = '<i class="fas fa-play"></i>';
            }


            // --- 6. 투두리스트 기능 ---
            function renderTodos() {
                containers.todoList.innerHTML = ''; 
                if (todos.length === 0) {
                    containers.todoList.innerHTML = '<p class="subtitle">오늘 할 일을 추가해보세요!</p>';
                }
                
                todos.forEach((todo, index) => {
                    const li = document.createElement('li');
                    li.className = `todo-item ${todo.done ? 'done' : ''}`;
                    li.dataset.index = index;
                    li.innerHTML = `
                        <i class="fas ${todo.done ? 'fa-check-circle' : 'fa-circle'}"></i>
                        <span>${todo.text}</span>
                        <button class="delete-btn"><i class="fas fa-trash"></i></button>
                    `;
                    
                    li.addEventListener('click', (e) => {
                        if (e.target.closest('.delete-btn')) return; 
                        toggleTodo(index);
                    });
                    
                    li.querySelector('.delete-btn').addEventListener('click', (e) => {
                        e.stopPropagation(); 
                        deleteTodo(index);
                    });
                    
                    containers.todoList.appendChild(li);
                });
            }

            function saveTodos() {
                localStorage.setItem('studyTodos', JSON.stringify(todos));
            }
            
            function loadTodos() {
                todos = JSON.parse(localStorage.getItem('studyTodos')) || [];
                renderTodos();
            }

            function addTodo(e) {
                e.preventDefault();
                const text = inputs.todoInput.value.trim();
                if (text === '') return;
                
                todos.push({ text: text, done: false });
                inputs.todoInput.value = '';
                saveTodos();
                renderTodos();
            }
            
            function toggleTodo(index) {
                todos[index].done = !todos[index].done;
                saveTodos();
                renderTodos();
            }
            
            function deleteTodo(index) {
                todos.splice(index, 1);
                saveTodos();
                renderTodos();
            }

            // --- 7. 로그 기록 & 차트 함수 ---
            
            function formatStudyTime(totalSeconds) {
                const h = Math.floor(totalSeconds / 3600);
                const m = Math.floor((totalSeconds % 3600) / 60);
                const s = totalSeconds % 60;
                let timeString = "";
                if (h > 0) timeString += `${h}시간 `;
                if (m > 0) timeString += `${m}분 `;
                if (s >= 0) timeString += `${s}초`;
                return timeString.trim() || "0초"; 
            }
            
            function formatStudyTimeMinutes(totalSeconds) {
                const m = Math.floor(totalSeconds / 60);
                return `${m}분`;
            }

            function renderTicket(container, log) {
                if (!container || !log) return; 
                const ticket = document.createElement('div');
                ticket.className = 'log-ticket';
                
                let icon = "fa-stopwatch";
                if (log.type === "Flight") icon = "fa-plane";
                if (log.type === "Pomodoro") icon = "fa-leaf";
                
                ticket.innerHTML = `
                    <div class="ticket-route"><i class="fas ${icon}"></i> ${log.route}</div>
                    <div class="ticket-duration">
                        <strong>총 공부 시간: ${formatStudyTime(log.duration)}</strong>
                    </div>
                    <div class="ticket-date">기록일: ${log.date}</div>
                `;
                if (container === containers.resultTicket) {
                    container.innerHTML = '';
                    container.appendChild(ticket);
                } else {
                    container.prepend(ticket); // 로그 목록은 맨 위에 추가
                }
            }

            function loadLog() {
                const logs = JSON.parse(localStorage.getItem('studyLog')) || [];
                
                containers.logList.innerHTML = ''; 
                containers.logChart.innerHTML = ''; 
                containers.logSummary.innerHTML = ''; 
                
                if (logs.length === 0) {
                    containers.logList.innerHTML = '<p>아직 기록이 없습니다.</p>';
                    containers.logSummary.innerHTML = '<strong>0초</strong>';
                    return;
                }
                
                let totalDuration = 0;
                logs.forEach(log => totalDuration += log.duration);
                containers.logSummary.innerHTML = `<strong>${formatStudyTime(totalDuration)}</strong>`;
                
                const recentLogs = logs.slice(-10);
                let maxDuration = 0;
                recentLogs.forEach(log => {
                    if (log.duration > maxDuration) maxDuration = log.duration;
                });
                
                if (maxDuration === 0) maxDuration = 1; 
                
                recentLogs.forEach(log => {
                    const bar = document.createElement('div');
                    bar.className = 'chart-bar';
                    const barHeight = (log.duration / maxDuration) * 100;
                    bar.style.height = `${barHeight < 5 ? 5 : barHeight}%`; 
                    
                    const tooltip = document.createElement('span');
                    tooltip.textContent = formatStudyTimeMinutes(log.duration); 
                    bar.appendChild(tooltip);
                    
                    containers.logChart.appendChild(bar);
                });

                logs.reverse().forEach(log => renderTicket(containers.logList, log));
            }

            // 로그 저장 타입 추가
            function saveLog(type, route, durationSeconds) {
                const logs = JSON.parse(localStorage.getItem('studyLog')) || [];
                if (durationSeconds <= 0) return null; 
                
                const newLog = { 
                    type, // 'Flight', 'Pomodoro', 'Stopwatch'
                    route, 
                    duration: durationSeconds, 
                    date: new Date().toLocaleDateString('ko-KR') 
                };
                logs.push(newLog);
                localStorage.setItem('studyLog', JSON.stringify(logs));
                return newLog; 
            }

            // --- 8. 이벤트 리스너 연결 ---

            buttons.start.addEventListener('click', () => showScreen('flightConfig')); // 비행 설정으로
            buttons.simpleStudy.addEventListener('click', () => showScreen('simpleStudy'));
            buttons.todo.addEventListener('click', () => { 
                loadTodos();
                showScreen('checklist');
            });
            buttons.viewLog.addEventListener('click', () => {
                loadLog(); 
                showScreen('log');
            });
            buttons.goHome.addEventListener('click', () => showScreen('start'));
            
            // 비행 설정 화면
            inputs.departureSelect.addEventListener('change', updateFlightConfigScreen);
            inputs.destinationSelect.addEventListener('change', updateFlightConfigScreen);
            buttons.goToBoardingPass.addEventListener('click', goToBoardingPass); // [수정] 좌석->탑승권
            buttons.boardPlane.addEventListener('click', startBoardingAnimation); // 탑승권->애니메이션
            
            buttons.focusContinue.addEventListener('click', hideFocusCheck);
            inputs.todoForm.addEventListener('submit', addTodo); 

            // 모든 뒤로가기 버튼 -> 홈으로 이동 (확인 포함)
            document.querySelectorAll('.back-button').forEach(button => {
                 button.addEventListener('click', (e) => {
                     const targetScreen = e.currentTarget.dataset.target; // 목표 화면 (대부분 'start')
                     
                    if (timerInterval || pomoIsRunning || stopwatchIsRunning) {
                         if (confirm("공부를 중단하고 홈으로 돌아가시겠습니까? (현재까지의 공부 시간은 기록됩니다)")) {
                             if (timerInterval) landImmediately(); 
                             else { 
                                 if (pomoIsRunning) resetPomo(); 
                                 if (stopwatchIsRunning) resetStopwatch(); 
                                 showScreen('start');
                             }
                         }
                    } 
                     else { // 타이머 실행 중 아니면 바로 목표 화면으로
                         showScreen(targetScreen);
                     }
                 });
            });
            
            seatMapButtons.forEach(button => {
                 // 좌석 선택 후 바로 비행 시작
                button.addEventListener('click', () => startFlight());
            });

            buttons.land.addEventListener('click', landImmediately);
            
            soundControlButtons.forEach(button => {
                button.addEventListener('click', (e) => {
                    soundControlButtons.forEach(btn => btn.classList.remove('active'));
                    e.currentTarget.classList.add('active');
                    stopAllSounds();
                    const soundType = e.currentTarget.dataset.sound;
                    if (soundType === 'inflight') {
                        currentSound = audio.inflight;
                         // 비행 중이고 + 소리 힌트가 안 보이는 상태(즉, 자동재생 성공 또는 클릭 후)일 때만 재생
                         if (timerInterval && textElements.audioHint.style.display !== 'block') {
                              currentSound.play().catch(err => console.log("Sound error:", err)); 
                         }
                    } else if (soundType === 'rain') {
                        currentSound = audio.rain;
                        if (timerInterval && textElements.audioHint.style.display !== 'block') {
                            currentSound.play().catch(err => console.log("Sound error:", err)); 
                        }
                    } else {
                        currentSound = null; // 'mute'
                    }
                });
            });

            // 기본 타이머 탭
            buttons.pomoTab.addEventListener('click', () => {
                buttons.pomoTab.classList.add('active');
                buttons.stopwatchTab.classList.remove('active');
                containers.pomoTimer.classList.add('active');
                containers.stopwatchTimer.classList.remove('active');
            });
            
            buttons.stopwatchTab.addEventListener('click', () => { // [복구]
                buttons.pomoTab.classList.remove('active');
                buttons.stopwatchTab.classList.add('active');
                containers.pomoTimer.classList.remove('active');
                containers.stopwatchTimer.classList.add('active');
            });
            
            // 기본 타이머 컨트롤
            buttons.pomoStartPause.addEventListener('click', startPausePomo);
            buttons.pomoReset.addEventListener('click', resetPomo);
            buttons.stopwatchStartPause.addEventListener('click', startPauseStopwatch); // [복구]
            buttons.stopwatchReset.addEventListener('click', resetStopwatch);         // [복구]
            
            // 뽀모도로 H:M:S 입력 시 리셋 (input 이벤트로 변경하여 실시간 반영)
            inputs.pomoStudyHours.addEventListener('input', resetPomo);
            inputs.pomoStudyMinutes.addEventListener('input', resetPomo);
            inputs.pomoStudySeconds.addEventListener('input', resetPomo);
            inputs.pomoBreakHours.addEventListener('input', resetPomo);
            inputs.pomoBreakMinutes.addEventListener('input', resetPomo);
            inputs.pomoBreakSeconds.addEventListener('input', resetPomo);

            // --- 9. 초기화 ---
            resetTrip(); 
            resetPomo(); 
            resetStopwatch(); 
        });
    </script>
    </body>
</html>
