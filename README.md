<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Расчёт теоретического расхода — Litum</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #0d6efd;
            --light-bg: #f5f7fa;
            --card-bg: white;
            --text: #333;
            --border: #ccc;
            --shadow: 0 4px 20px rgba(0,0,0,0.12);
            --accent: #0d6efd;
            --accent-hover: #0b5ed7;
            --input-focus: rgba(13, 110, 253, 0.15);
            --result-bg: #f8f9fa;
            --result-border: #0d6efd;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Roboto', Arial, sans-serif;
            background: var(--light-bg);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            min-height: 100vh;
        }

        .container {
            background: var(--card-bg);
            border-radius: 12px;
            box-shadow: var(--shadow);
            padding: 28px;
            max-width: 1200px;
            width: 100%;
        }

        .logo {
            text-align: center;
            margin-bottom: 24px;
        }

        .logo img {
            max-height: 140px;
            display: block;
            margin: 0 auto 16px;
            object-fit: contain;
        }

        .logo .subtitle {
            font-size: 14px;
            color: #666;
            margin-top: 4px;
        }

        .form-section {
            background: #f8f9fa;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 24px;
        }

        .section-title {
            font-size: 18px;
            color: #333;
            margin-bottom: 16px;
            padding-bottom: 8px;
            border-bottom: 1px solid #eee;
            font-weight: 500;
        }

        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 16px;
        }

        label {
            display: block;
            margin: 12px 0 6px;
            font-weight: 500;
            color: var(--text);
            font-size: 14px;
        }

        input {
            width: 100%;
            padding: 12px 12px;
            border: 1px solid var(--border);
            border-radius: 8px;
            font-size: 15px;
            box-sizing: border-box;
            transition: border-color 0.2s;
            font-family: inherit;
        }

        input:focus {
            outline: none;
            border-color: var(--accent);
            box-shadow: 0 0 0 3px var(--input-focus);
        }

        button {
            width: 100%;
            padding: 14px;
            background: var(--accent);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            margin-top: 20px;
            transition: background 0.2s;
        }

        button:hover {
            background: var(--accent-hover);
        }

        .results-section {
            background: var(--result-bg);
            border-radius: 8px;
            padding: 16px;
            border-left: 4px solid var(--result-border);
            font-size: 15px;
            line-height: 1.5;
        }

        .results-section h2 {
            margin-top: 0;
            font-size: 18px;
            color: #333;
            padding-bottom: 8px;
            border-bottom: 1px solid #eee;
        }

        .result-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px solid #eee;
        }

        .result-label {
            font-weight: 500;
            color: #333;
        }

        .result-value {
            font-weight: 700;
            color: #0d4a7f;
        }

        /* Адаптация под разные размеры экрана */
        @media (min-width: 1024px) {
            .form-grid {
                grid-template-columns: repeat(3, 1fr); /* 3 колонки на больших экранах */
            }
        }

        @media (max-width: 1023px) and (min-width: 768px) {
            .form-grid {
                grid-template-columns: repeat(2, 1fr); /* 2 колонки на планшетах */
            }
        }

        @media (max-width: 767px) {
            .form-grid {
                grid-template-columns: 1fr; /* 1 колонка на мобильных */
            }

            .container {
                padding: 16px;
            }

            .logo img {
                max-height: 120px;
            }

            .logo .subtitle {
                font-size: 13px;
            }

            .section-title {
                font-size: 16px;
            }

            .result-item {
                flex-direction: column;
            }

            .result-value {
                margin-top: 4px;
            }
        }

        @media (max-width: 480px) {
            body {
                padding: 12px;
            }

            .container {
                padding: 16px;
            }

            .logo img {
                max-height: 100px;
            }

            .section-title {
                font-size: 15px;
            }

            input {
                padding: 12px 10px;
                font-size: 16px; /* Увеличен шрифт для удобства */
            }

            button {
                padding: 16px;
                font-size: 16px;
            }

            .result-value {
                font-size: 14px;
            }
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="logo">
            <img src="Litum logo.png" alt="Litum logo">
            <div class="subtitle">Расчёт теоретического расхода краски</div>
        </div>

        <section class="form-section">
            <h2 class="section-title">📊 Данные для расчета</h2>
            
            <div class="form-grid">
                <div class="form-group">
                    <label for="dft">Толщина сухой плёнки (DFT), мкм:</label>
                    <input type="number" id="dft" value="34" step="0.1" placeholder="Например: 34" inputmode="decimal">
                </div>

                <div class="form-group">
                    <label for="mass_blank">Вес стекла без краски (г):</label>
                    <input type="number" id="mass_blank" value="54.5547" step="0.0001" placeholder="Например: 54.5547" inputmode="decimal">
                </div>

                <div class="form-group">
                    <label for="mass_wet">Вес стекла с мокрой краской (г):</label>
                    <input type="number" id="mass_wet" value="55.0467" step="0.0001" placeholder="Например: 55.0467" inputmode="decimal">
                </div>

                <div class="form-group">
                    <label for="mass_dry">Вес стекла с сухой краской (г):</label>
                    <input type="number" id="mass_dry" value="54.8940" step="0.0001" placeholder="Например: 54.8940" inputmode="decimal">
                </div>

                <div class="form-group">
                    <label for="length_mm">Длина пленки (мм):</label>
                    <input type="number" id="length_mm" value="100" step="0.1" placeholder="Например: 100" inputmode="decimal">
                </div>

                <div class="form-group">
                    <label for="width_mm">Ширина пленки (мм):</label>
                    <input type="number" id="width_mm" value="64.65" step="0.1" placeholder="Например: 64.65" inputmode="decimal">
                </div>

                <div class="form-group">
                    <label for="ms_percent">МДНВ (для 2-комп. смесь А+В), %:</label>
                    <input type="number" id="ms_percent" value="66.61" step="0.01" min="0" max="100" placeholder="Например: 66.61" inputmode="decimal">
                </div>

                <div class="form-group">
                    <label for="rho_wet">Плотность краски (для 2-комп. смесь А+В), г/см³:</label>
                    <input type="number" id="rho_wet" value="1.269" step="0.001" placeholder="Например: 1.269" inputmode="decimal">
                </div>

                <div class="form-group">
                    <label for="contrast">Коэффициент контрастности (0–1):</label>
                    <input type="number" id="contrast" value="0.87" step="0.01" min="0" max="1" placeholder="Например: 0.87" inputmode="decimal">
                </div>
            </div>

            <button onclick="calculate()">🚀 Рассчитать</button>
        </section>

        <section class="results-section">
            <h2>📈 Результаты расчёта</h2>
            <div id="result">
                <div class="result-item">
                    <span class="result-label">Введите данные и нажмите "Рассчитать"</span>
                </div>
            </div>
        </section>
    </div>

    <script>
        function calculate() {
            const dft = parseFloat(document.getElementById('dft').value);
            const mass_blank = parseFloat(document.getElementById('mass_blank').value);
            const mass_wet_full = parseFloat(document.getElementById('mass_wet').value);
            const mass_dry_full = parseFloat(document.getElementById('mass_dry').value);
            const L_mm = parseFloat(document.getElementById('length_mm').value);
            const W_mm = parseFloat(document.getElementById('width_mm').value);
            const ms_percent = parseFloat(document.getElementById('ms_percent').value);
            const rho_wet = parseFloat(document.getElementById('rho_wet').value);
            const contrast = parseFloat(document.getElementById('contrast').value);

            if (isNaN(dft) || isNaN(mass_blank) || isNaN(mass_wet_full) || isNaN(mass_dry_full) || isNaN(L_mm) || isNaN(W_mm) || isNaN(ms_percent) || isNaN(rho_wet) || isNaN(contrast) ||
                dft <= 0 || mass_blank <= 0 || mass_wet_full <= mass_blank || mass_dry_full <= mass_blank || L_mm <= 0 || W_mm <= 0 || ms_percent < 0 || ms_percent > 100 || rho_wet <= 0 || contrast < 0 || contrast > 1) {
                document.getElementById('result').innerHTML = `
                    <div class="result-item">
                        <span class="result-label">⚠️ Ошибка:</span>
                        <span class="result-value">Пожалуйста, введите корректные данные.</span>
                    </div>
                `;
                return;
            }

            // Рассчитываем массу мокрой и сухой плёнки
            const m_wet = mass_wet_full - mass_blank;
            const m_dry = mass_dry_full - mass_blank;

            // Переводим мм в см
            const L_cm = L_mm / 10;
            const W_cm = W_mm / 10;
            const A = L_cm * W_cm; // площадь в см²

            const V_dry = (A * dft) / 10000; // объём сухой плёнки в см³
            const rho_dry = m_dry / V_dry; // плотность сухой плёнки (внутри используется)

            // Преобразуем процент в долю
            const ms = ms_percent / 100;

            const vs_percent = (ms / rho_dry) * rho_wet * 100; // объёмное содержание сухого остатка в %

            // Расход без учёта контрастности
            const R_no_contrast = dft / (10 * vs_percent);

            // Расход с учётом контрастности
            const R_with_contrast = R_no_contrast / contrast;

            // Покрытие без и с контрастностью
            const coverage_no_contrast_m2_per_l = 1 / R_no_contrast;
            const coverage_with_contrast_m2_per_l = 1 / R_with_contrast;

            const coverage_no_contrast_m2_per_kg = 1 / (R_no_contrast * rho_wet);
            const coverage_with_contrast_m2_per_kg = 1 / (R_with_contrast * rho_wet);

            document.getElementById('result').innerHTML = `
                <div class="result-item">
                    <span class="result-label">Масса мокрой плёнки:</span>
                    <span class="result-value">${m_wet.toFixed(4)} г</span>
                </div>
                <div class="result-item">
                    <span class="result-label">Масса сухой плёнки:</span>
                    <span class="result-value">${m_dry.toFixed(4)} г</span>
                </div>
                <div class="result-item">
                    <span class="result-label">Площадь пленки:</span>
                    <span class="result-value">${A.toFixed(2)} см²</span>
                </div>
                <div class="result-item">
                    <span class="result-label">Расчетное ОДНВ:</span>
                    <span class="result-value">${vs_percent.toFixed(2)}%</span>
                </div>
                <div class="result-item">
                    <span class="result-label">Расход без учёта контрастности:</span>
                    <span class="result-value">${R_no_contrast.toFixed(4)} л/м², ${coverage_no_contrast_m2_per_l.toFixed(2)} м²/л, ${coverage_no_contrast_m2_per_kg.toFixed(2)} м²/кг</span>
                </div>
                <div class="result-item">
                    <span class="result-label">Расход с учётом контрастности:</span>
                    <span class="result-value">${R_with_contrast.toFixed(4)} л/м², ${coverage_with_contrast_m2_per_l.toFixed(2)} м²/л, ${coverage_with_contrast_m2_per_kg.toFixed(2)} м²/кг</span>
                </div>
            `;
        }

        // Автоматически рассчитать при загрузке страницы
        window.onload = calculate;
    </script>

</body>
</html>
