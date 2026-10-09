<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>СберБизнес | Панель Сотрудника ЭДО</title>
    <style>
        :root {
            --bg-main: #f4f6f8;
            --bg-card: #ffffff;
            --border-color: #e4e7eb;
            --border-hover: #cfd4dc;
            --text-dark: #1f2229;
            --text-secondary: #707684;
            --text-tertiary: #9ea5b1;
            --sber-green: #21a038;
            --sber-green-hover: #1b892f;
            --sber-green-light: #e8f7ec;
            --danger-red: #f03d3d;
            --danger-bg: #fee8e8;
            --sidebar-width: 240px;
            --header-height: 64px;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; }
        body { background-color: var(--bg-main); color: var(--text-dark); display: flex; min-height: 100vh; overflow-x: hidden; }

        /* SVG ИКОНКИ */
        .icon { width: 20px; height: 20px; display: inline-flex; align-items: center; justify-content: center; stroke-width: 1.8; stroke: currentColor; fill: none; stroke-linecap: round; stroke-linejoin: round; }
        .icon-sm { width: 16px; height: 16px; }
        .icon-lg { width: 26px; height: 26px; }

        /* САЙДБАР */
        .sidebar { width: var(--sidebar-width); background: var(--bg-card); border-right: 1px solid var(--border-color); display: flex; flex-direction: column; position: fixed; top: 0; bottom: 0; left: 0; z-index: 100; }
        .brand-header { height: var(--header-height); display: flex; align-items: center; padding: 0 20px; gap: 10px; border-bottom: 1px solid transparent; }
        .sber-circle-logo { width: 28px; height: 28px; border-radius: 50%; background: linear-gradient(135deg, #21a038 0%, #157927 100%); display: flex; align-items: center; justify-content: center; color: #fff; font-weight: 900; font-size: 14px; }
        .brand-title { font-size: 17px; font-weight: 700; color: var(--text-dark); letter-spacing: -0.3px; }
        .brand-title span { color: var(--sber-green); }

        .btn-create-main { margin: 12px 16px; background: var(--bg-main); border: 1px solid var(--border-color); border-radius: 12px; padding: 12px 14px; display: flex; align-items: center; gap: 10px; font-weight: 600; font-size: 14px; cursor: pointer; transition: 0.2s; color: var(--text-dark); }
        .btn-create-main:hover { background: #eaedf1; border-color: var(--border-hover); }
        .btn-create-main .plus-circle { width: 22px; height: 22px; border-radius: 50%; background: var(--sber-green); color: #fff; display: flex; align-items: center; justify-content: center; font-size: 15px; }

        .nav-list { list-style: none; padding: 0 8px; flex: 1; overflow-y: auto; display: flex; flex-direction: column; gap: 2px; }
        .nav-item { display: flex; align-items: center; gap: 12px; padding: 11px 14px; border-radius: 10px; font-size: 13.5px; font-weight: 500; color: var(--text-secondary); cursor: pointer; transition: 0.15s; }
        .nav-item:hover { background: #f4f6f8; color: var(--text-dark); }
        .nav-item.active { background: var(--sber-green-light); color: var(--sber-green); font-weight: 600; }
        .nav-item.active .icon { stroke: var(--sber-green); }

        /* ОСНОВНОЙ КОНТЕНТ */
        .main-wrapper { margin-left: var(--sidebar-width); flex: 1; display: flex; flex-direction: column; min-width: 0; }

        /* ШАПКА ХЕДЕР */
        .header { height: var(--header-height); background: var(--bg-card); border-bottom: 1px solid var(--border-color); display: flex; align-items: center; justify-content: space-between; padding: 0 28px; position: sticky; top: 0; z-index: 90; }
        .header-search { display: flex; align-items: center; gap: 10px; background: var(--bg-main); border: 1px solid transparent; border-radius: 20px; padding: 8px 18px; width: 340px; color: var(--text-secondary); }
        .header-search input { border: none; background: transparent; outline: none; width: 100%; font-size: 13.5px; color: var(--text-dark); }
        
        .header-right { display: flex; align-items: center; gap: 22px; }
        .header-stat { display: flex; align-items: center; gap: 10px; font-size: 13px; text-align: right; }
        .header-stat .amount { font-weight: 700; color: var(--text-dark); font-size: 14px; }
        .header-stat .sub { font-size: 11px; color: var(--text-tertiary); }
        .sync-btn { border: none; background: transparent; cursor: pointer; color: var(--text-secondary); display: flex; align-items: center; transition: transform 0.3s; }
        .sync-btn:hover { transform: rotate(90deg); color: var(--text-dark); }

        .profile-btn { display: flex; align-items: center; gap: 10px; background: none; border: none; cursor: pointer; text-align: left; padding: 4px 8px; border-radius: 8px; }
        .profile-btn:hover { background: var(--bg-main); }
        .profile-avatar { width: 34px; height: 34px; border-radius: 50%; background: #e5e9f0; display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 13px; color: var(--text-dark); }

        /* СТРАНИЦЫ И ВИДЖЕТЫ */
        .content { padding: 24px 28px; flex: 1; }
        .page { display: none; }
        .page.active { display: block; animation: fadeIn 0.25s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(4px); } to { opacity: 1; transform: translateY(0); } }

        .dashboard-grid { display: grid; grid-template-columns: 2fr 1fr; gap: 20px; margin-bottom: 24px; }
        .card { background: var(--bg-card); border-radius: 16px; border: 1px solid var(--border-color); padding: 22px; position: relative; }
        
        .card-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; }
        .card-title { font-size: 16px; font-weight: 700; color: var(--text-dark); display: flex; align-items: center; gap: 8px; }

        .accounts-box { display: flex; justify-content: space-between; align-items: center; padding: 14px 18px; background: var(--bg-main); border-radius: 12px; margin-bottom: 14px; }
        .accounts-info .acc-num { font-size: 12px; color: var(--text-secondary); }
        .accounts-info .acc-bal { font-size: 22px; font-weight: 800; color: var(--text-dark); margin-top: 2px; }

        .tasks-box { text-align: center; padding: 30px 20px; }
        .tasks-icon-wrap { width: 48px; height: 48px; border-radius: 50%; background: var(--sber-green-light); color: var(--sber-green); display: inline-flex; align-items: center; justify-content: center; margin-bottom: 12px; }

        .side-banners { display: flex; flex-direction: column; gap: 12px; }
        .banner-card { padding: 16px; border-radius: 14px; border: 1px solid var(--border-color); background: #fff; cursor: pointer; transition: 0.2s; position: relative; overflow: hidden; display: flex; gap: 14px; align-items: center; }
        .banner-card:hover { border-color: var(--sber-green); transform: translateY(-1px); box-shadow: 0 4px 12px rgba(0,0,0,0.03); }
        .banner-green { background: linear-gradient(135deg, #107c29 0%, #0c5c1e 100%); color: #fff; border: none; }
        .banner-green:hover { border: none; }
        .badge-tag { background: #ffe9d6; color: #d96200; font-size: 10.5px; font-weight: 700; padding: 3px 6px; border-radius: 4px; display: inline-block; margin-top: 4px; }

        /* ТАБЛИЦЫ */
        .table-filter-bar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; flex-wrap: wrap; gap: 12px; }
        .filter-tags { display: flex; gap: 8px; flex-wrap: wrap; }
        .filter-tag { padding: 6px 12px; border-radius: 8px; font-size: 12px; font-weight: 500; background: var(--bg-main); color: var(--text-secondary); border: 1px solid var(--border-color); cursor: pointer; display: flex; align-items: center; gap: 6px; }
        .filter-tag.active { background: #1f2229; color: #fff; border-color: #1f2229; }

        .table-container { overflow-x: auto; border: 1px solid var(--border-color); border-radius: 12px; background: #fff; }
        table { width: 100%; border-collapse: collapse; text-align: left; font-size: 13.5px; }
        th { padding: 14px 16px; background: #fafbfc; border-bottom: 1px solid var(--border-color); font-weight: 600; color: var(--text-secondary); font-size: 12px; }
        td { padding: 14px 16px; border-bottom: 1px solid var(--border-color); vertical-align: middle; }
        tr:last-child td { border-bottom: none; }
        tr:hover td { background-color: #fafbfc; }

        .status-dot { width: 8px; height: 8px; border-radius: 50%; display: inline-block; margin-right: 6px; }
        .status-badge { display: inline-flex; align-items: center; font-size: 12.5px; font-weight: 600; }
        .status-success { color: var(--sber-green); }
        .status-success .status-dot { background: var(--sber-green); }
        .status-danger { color: var(--danger-red); }
        .status-danger .status-dot { background: var(--danger-red); }

        /* КНОПКИ И ПОЛЯ */
        .btn { border: none; border-radius: 8px; padding: 10px 18px; font-size: 13.5px; font-weight: 600; cursor: pointer; transition: 0.15s; display: inline-flex; align-items: center; justify-content: center; gap: 8px; }
        .btn-primary { background: var(--sber-green); color: #fff; }
        .btn-primary:hover { background: var(--sber-green-hover); }
        .btn-outline { background: transparent; border: 1px solid var(--border-color); color: var(--text-dark); }
        .btn-outline:hover { background: var(--bg-main); }
        .btn-danger { background: var(--danger-red); color: #fff; }
        .btn-danger:hover { background: #d02f2f; }

        .input-group { margin-bottom: 16px; }
        .input-group label { display: block; font-size: 12px; font-weight: 600; color: var(--text-secondary); margin-bottom: 6px; }
        .input-control { width: 100%; padding: 11px 14px; border: 1px solid var(--border-color); border-radius: 8px; font-size: 14px; outline: none; transition: 0.2s; background: #fff; color: var(--text-dark); }
        .input-control:focus { border-color: var(--sber-green); box-shadow: 0 0 0 3px rgba(33, 160, 56, 0.15); }

        .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }

        /* ДРОПЗОНА */
        .file-zone { border: 2px dashed var(--border-color); border-radius: 12px; padding: 24px; text-align: center; cursor: pointer; transition: 0.2s; background: var(--bg-main); position: relative; }
        .file-zone:hover { border-color: var(--sber-green); background: var(--sber-green-light); }
        .file-zone.loaded { border-color: var(--sber-green); background: #f0faf3; }
        .file-zone input[type="file"] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }

        /* ХОЛСТ */
        .canvas-box { border: 1px solid var(--border-color); border-radius: 10px; overflow: hidden; background: #fff; margin-bottom: 12px; }
        canvas { display: block; width: 100%; height: 180px; cursor: crosshair; }

        /* ПОДПИСКИ КАРТОЧКИ */
        .subs-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 16px; }
        .sub-card { border: 1px solid var(--border-color); border-radius: 12px; padding: 18px; display: flex; align-items: flex-start; gap: 14px; background: #fff; cursor: pointer; transition: 0.2s; }
        .sub-card:hover { border-color: var(--sber-green); box-shadow: 0 4px 16px rgba(0,0,0,0.04); }
        .sub-card input { width: 20px; height: 20px; accent-color: var(--sber-green); margin-top: 2px; }
        .sub-icon-wrap { width: 40px; height: 40px; border-radius: 10px; background: var(--bg-main); display: flex; align-items: center; justify-content: center; color: var(--sber-green); flex-shrink: 0; }

        /* МОДАЛЬНЫЕ ОКНА */
        .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.45); display: flex; align-items: center; justify-content: center; z-index: 999; backdrop-filter: blur(2px); }
        .modal-card { background: #fff; width: 100%; max-width: 440px; border-radius: 16px; padding: 28px; box-shadow: 0 10px 40px rgba(0,0,0,0.15); animation: zoomIn 0.2s ease-out; }
        @keyframes zoomIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }

        /* ДОКУМЕНТ А4 */
        #docOverlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.7); z-index: 1000; overflow-y: auto; padding: 30px; }
        .a4-sheet { background: #fff; width: 100%; max-width: 800px; min-height: 1120px; margin: 0 auto; padding: 60px 70px; font-family: "Times New Roman", Times, serif; color: #000; box-shadow: 0 10px 30px rgba(0,0,0,0.3); }
        .a4-header { text-align: center; font-size: 20px; font-weight: bold; margin-bottom: 30px; text-transform: uppercase; }
        .a4-body { font-size: 15px; line-height: 1.6; text-align: justify; white-space: pre-wrap; margin-bottom: 50px; }
        .a4-sigs { display: flex; justify-content: space-between; margin-top: 60px; }
        .a4-sig-block { width: 45%; }
        .a4-sig-img { height: 75px; max-width: 100%; object-fit: contain; border-bottom: 1px solid #000; margin-bottom: 6px; }
        .doc-bar { position: fixed; bottom: 25px; left: 50%; transform: translateX(-50%); background: #fff; padding: 12px 20px; border-radius: 12px; box-shadow: 0 8px 30px rgba(0,0,0,0.25); display: flex; gap: 12px; }

        @media print {
            body * { visibility: hidden; }
            #docOverlay, #docOverlay * { visibility: visible; }
            #docOverlay { position: absolute; left: 0; top: 0; padding: 0; background: none; }
            .a4-sheet { box-shadow: none; padding: 20px; max-width: 100%; }
            .doc-bar { display: none !important; }
        }
    </style>
</head>
<body>

    <!-- МОДАЛЬНОЕ ОКНО: ВВОД ФИО СОТРУДНИКА ПРИ ВХОДЕ -->
    <div id="authModal" class="modal-overlay">
        <div class="modal-card">
            <div style="display:flex; align-items:center; gap:10px; margin-bottom:18px;">
                <div class="sber-circle-logo">С</div>
                <div style="font-weight:700; font-size:18px;">СберБизнес ЭДО</div>
            </div>
            <div style="color:var(--text-secondary); font-size:13.5px; margin-bottom:20px;">Авторизация сотрудника отделения в защищённом контуре документооборота.</div>
            
            <div class="input-group">
                <label>ФИО Сотрудника:</label>
                <input type="text" id="authFioInput" class="input-control" placeholder="Иванов Иван Иванович" value="Бондарева Анастасия Александровна">
            </div>
            <div class="input-group">
                <label>Должность / Филиал:</label>
                <input type="text" id="authRoleInput" class="input-control" value="Ведущий специалист ЭДО, Воронежское отд. №9013">
            </div>
            <button class="btn btn-primary" style="width:100%; margin-top:8px;" onclick="saveEmployeeAuth()">
                Войти в рабочее место
            </button>
        </div>
    </div>

    <!-- САЙДБАР (ИКОНКИ ВМЕСТО СМАЙЛИКОВ) -->
    <aside class="sidebar">
        <div class="brand-header">
            <div class="sber-circle-logo">
                <svg class="icon icon-sm" viewBox="0 0 24 24"><path d="M5 13l4 4L19 7"/></svg>
            </div>
            <div class="brand-title"><span>СБЕР</span> Бизнес</div>
        </div>

        <button class="btn-create-main" onclick="switchNav('page-issue')">
            <div class="plus-circle">+</div>
            <span>Создать ЭЦП</span>
        </button>

        <ul class="nav-list">
            <li class="nav-item active" onclick="switchNav('page-home', this)">
                <svg class="icon" viewBox="0 0 24 24"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/></svg>
                <span>Главная</span>
            </li>
            <li class="nav-item" onclick="switchNav('page-payments', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/></svg>
                <span>Платежи и переводы</span>
            </li>
            <li class="nav-item" onclick="switchNav('page-issue', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"/></svg>
                <span>Выпуск и отзыв ЭЦП</span>
            </li>
            <li class="nav-item" onclick="switchNav('page-subs', this)">
                <svg class="icon" viewBox="0 0 24 24"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
                <span>Сервисы и подписки</span>
            </li>
            <li class="nav-item" onclick="switchNav('page-docs', this)">
                <svg class="icon" viewBox="0 0 24 24"><polyline points="6 9 6 2 18 2 18 9"/><path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"/><rect x="6" y="14" width="12" height="8"/></svg>
                <span>Печать документов</span>
            </li>
            <li class="nav-item" onclick="switchNav('page-verify', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg>
                <span>Проверка сроков</span>
            </li>
            <li class="nav-item" onclick="openAuthModal()">
                <svg class="icon" viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
                <span>Сменить оператора</span>
            </li>
        </ul>
    </aside>

    <!-- ОСНОВНАЯ ЧАСТЬ -->
    <div class="main-wrapper">
        
        <!-- ХЕДЕР (КАК НА ФОТО СБЕРБИЗНЕС) -->
        <header class="header">
            <div class="header-search">
                <svg class="icon icon-sm" viewBox="0 0 24 24"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
                <input type="text" placeholder="Поиск по документам или клиентам...">
            </div>

            <div class="header-right">
                <div class="header-stat">
                    <div>
                        <div class="amount" id="topBalance">35 719,39 ₽</div>
                        <div class="sub">На рублевых счетах, 18:36</div>
                    </div>
                    <button class="sync-btn" onclick="syncApp()" title="Синхронизировать">
                        <svg class="icon" viewBox="0 0 24 24"><polyline points="23 4 23 10 17 10"/><polyline points="1 20 1 14 7 14"/><path d="M3.51 9a9 9 0 0 1 14.85-3.36L23 10M1 14l4.64 4.36A9 9 0 0 0 20.49 15"/></svg>
                    </button>
                </div>

                <div class="profile-btn" onclick="openAuthModal()" title="Нажмите для смены профиля">
                    <div class="profile-avatar" id="headerAvatar">БА</div>
                    <div>
                        <div style="font-weight:700; font-size:13px;" id="headerFioDisplay">Бондарева А. А.</div>
                        <div style="font-size:11px; color:var(--text-secondary);" id="headerRoleDisplay">Сотрудник ЭДО</div>
                    </div>
                </div>
            </div>
        </header>

        <!-- КОНТЕНТНЫЕ ОБЛАСТИ -->
        <main class="content">

            <!-- 1. СТРАНИЦА "ГЛАВНАЯ" (ВИДЖЕТЫ КАК НА 1604.JPG) -->
            <div id="page-home" class="page active">
                <div class="dashboard-grid">
                    <div>
                        <!-- Карточка Счета -->
                        <div class="card" style="margin-bottom: 20px;">
                            <div class="card-header">
                                <div class="card-title">
                                    <span>Счета</span>
                                    <svg class="icon icon-sm" style="color:var(--text-tertiary);" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><line x1="12" y1="16" x2="12" y2="12"/><line x1="12" y1="8" x2="12.01" y2="8"/></svg>
                                </div>
                                <div style="font-size:12px; color:var(--text-tertiary);">28.09.2026, 18:36</div>
                            </div>
                            
                            <div class="accounts-box">
                                <div class="accounts-info">
                                    <div class="acc-num">Расчётный счет •••• 7733</div>
                                    <div class="acc-bal">35 719,39 ₽</div>
                                </div>
                                <button class="btn btn-outline" style="font-size:12px; padding:6px 12px;">Реквизиты</button>
                            </div>

                            <div style="display:flex; justify-content:space-between; align-items:center; font-size:13px; color:var(--text-secondary);">
                                <div>Откладывать 6% от каждого поступления: <b>Счёт-копилка</b></div>
                                <div style="display:flex; gap:10px;">
                                    <button class="btn btn-outline" style="padding:6px 12px; font-size:12px;">Новый счет</button>
                                    <button class="btn btn-primary" style="padding:6px 12px; font-size:12px;">Выписка</button>
                                </div>
                            </div>
                        </div>

                        <!-- Карточка Мои дела -->
                        <div class="card">
                            <div class="card-header">
                                <div class="card-title">Мои дела</div>
                                <div style="font-size:12px; color:var(--text-tertiary);">ДД.ММ.ГГГГ</div>
                            </div>
                            <div class="tasks-box">
                                <div class="tasks-icon-wrap">
                                    <svg class="icon icon-lg" viewBox="0 0 24 24"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/><path d="M9 16l2 2 4-4"/></svg>
                                </div>
                                <div style="font-weight:700; font-size:15px; margin-bottom:4px;">На сегодня всё сделано.</div>
                                <div style="color:var(--text-secondary); font-size:13px; margin-bottom:16px;">А пока создайте новое дело или выпустите подпись клиенту.</div>
                                <div style="display:flex; justify-content:center; gap:10px;">
                                    <button class="btn btn-primary" onclick="switchNav('page-issue')">Создать дело</button>
                                    <button class="btn btn-outline">Все дела</button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Баннеры справа -->
                    <div class="side-banners">
                        <div class="banner-card">
                            <svg class="icon icon-lg" style="color:var(--sber-green);" viewBox="0 0 24 24"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/></svg>
                            <div>
                                <div style="font-weight:700; font-size:13.5px;">Продавайте больше с QR-кодом</div>
                                <div style="font-size:12px; color:var(--text-secondary);">Подключение за 1 день</div>
                            </div>
                        </div>

                        <div class="banner-card banner-green">
                            <svg class="icon icon-lg" viewBox="0 0 24 24"><polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"/></svg>
                            <div>
                                <div style="font-weight:700; font-size:13.5px;">Быстрые платежи в СберБизнес</div>
                                <div style="font-size:12px; opacity:0.85;">Мгновенная отправка клиентам</div>
                            </div>
                        </div>

                        <div class="banner-card">
                            <svg class="icon icon-lg" style="color:#2196f3;" viewBox="0 0 24 24"><line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg>
                            <div>
                                <div style="font-weight:700; font-size:13.5px;">Бухгалтерия Онлайн</div>
                                <span class="badge-tag">АКЦИЯ</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Лента операций (Таблица со статусами) -->
                <div class="card">
                    <div class="card-header">
                        <div class="card-title">Лента операций и реестр ЭДО</div>
                        <div class="filter-tags">
                            <span class="filter-tag active">От: 30.06.2026 ✕</span>
                            <span class="filter-tag active">До: 28.09.2026 ✕</span>
                            <span class="filter-tag">Все операции</span>
                        </div>
                    </div>

                    <div class="table-container">
                        <table id="operationsTable">
                            <thead>
                                <tr>
                                    <th>Номер и дата</th>
                                    <th>Контрагент / Клиент</th>
                                    <th>Документ / Действие</th>
                                    <th>Сумма</th>
                                    <th>Статус</th>
                                    <th style="text-align:right;">Действия</th>
                                </tr>
                            </thead>
                            <tbody id="operationsList">
                                <tr>
                                    <td>
                                        <div style="font-weight:700;">№ 78</div>
                                        <div style="font-size:11.5px; color:var(--text-tertiary);">28.09.2026, 16:42</div>
                                    </td>
                                    <td>
                                        <div style="font-weight:600;">БОНДАРЕВА АНАСТАСИЯ АЛЕКСАНДРОВНА</div>
                                        <div style="font-size:11.5px; color:var(--text-tertiary);">ИНН 366401928491</div>
                                    </td>
                                    <td>Платежное поручение (Тариф ЭДО)</td>
                                    <td style="font-weight:700;">-22 512,00 ₽</td>
                                    <td>
                                        <span class="status-badge status-success">
                                            <span class="status-dot"></span>Исполнено
                                        </span>
                                    </td>
                                    <td style="text-align:right;">
                                        <button class="btn btn-outline" style="padding:4px 8px;" onclick="alert('Печать чека...')">
                                            <svg class="icon icon-sm" viewBox="0 0 24 24"><polyline points="6 9 6 2 18 2 18 9"/><path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"/><rect x="6" y="14" width="12" height="8"/></svg>
                                        </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- 2. СТРАНИЦА "ПЛАТЕЖИ И ПЕРЕВОДЫ" (КАК НА 1605.JPG) -->
            <div id="page-payments" class="page">
                <div class="card-header" style="margin-bottom: 20px;">
                    <div>
                        <h2 style="font-size:22px; font-weight:800;">Платежи и переводы</h2>
                        <div style="color:var(--text-secondary); font-size:13px;">Управление подписанными реестрами и поручениями</div>
                    </div>
                    <button class="btn btn-primary" onclick="switchNav('page-issue')">+ Новый платёж</button>
                </div>

                <div class="table-filter-bar">
                    <div class="filter-tags">
                        <span class="filter-tag active">Все</span>
                        <span class="filter-tag">На подпись и отправку</span>
                        <span class="filter-tag">Исполненные</span>
                        <span class="filter-tag">Отклонённые</span>
                    </div>
                    <input type="text" class="input-control" placeholder="Фильтр по контрагенту..." style="width:260px; padding:6px 12px; font-size:12.5px;">
                </div>

                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th><input type="checkbox"></th>
                                <th>Номер</th>
                                <th>Дата</th>
                                <th>Контрагент</th>
                                <th>Назначение</th>
                                <th>Сумма</th>
                                <th>Статус</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td><input type="checkbox"></td>
                                <td>78</td>
                                <td>28.09.2026</td>
                                <td><b>БОНДАРЕВА АНАСТАСИЯ АЛЕКСАНДРОВНА</b><br><span style="font-size:11px; color:#888;">Счет: 40817 810 3 1300 5462518</span></td>
                                <td>Доход от предпринимательской деятельности. НДС не облагается.</td>
                                <td style="font-weight:700;">22 512,00 ₽</td>
                                <td><span class="status-badge status-success"><span class="status-dot"></span>Исполнен</span></td>
                            </tr>
                            <tr>
                                <td><input type="checkbox"></td>
                                <td>77</td>
                                <td>13.09.2026</td>
                                <td><b>ООО «СБЕРБИЗНЕС ТРЕЙД»</b><br><span style="font-size:11px; color:#888;">Счет: 40702 810 9 0000 1289123</span></td>
                                <td>Оплата услуг защищённого ЭДО за 3 квартал.</td>
                                <td style="font-weight:700;">500,00 ₽</td>
                                <td><span class="status-badge status-success"><span class="status-dot"></span>Исполнен</span></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- 3. ВЫПУСК И ПЕРЕВЫПУСК ЭЦП -->
            <div id="page-issue" class="page">
                <h2 style="font-size:22px; font-weight:800; margin-bottom:20px;">Генератор и выпуск ключей ЭЦП</h2>

                <div class="grid-2">
                    <div class="card">
                        <div class="card-title" style="margin-bottom:16px;">Данные владельца сертификата</div>
                        
                        <div class="input-group">
                            <label>ФИО Клиента:</label>
                            <input type="text" id="newFio" class="input-control" placeholder="Иванов Иван Иванович">
                        </div>

                        <div class="input-group">
                            <label>ИНН Клиента (12 знаков):</label>
                            <input type="text" id="newInn" class="input-control" placeholder="770012345678" maxlength="12">
                        </div>

                        <div class="input-group">
                            <label>Точный срок окончания действия подписи:</label>
                            <input type="datetime-local" id="newExpiry" class="input-control">
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-title" style="margin-bottom:16px;">Графическая факсимильная подпись</div>
                        <div class="canvas-box">
                            <canvas id="issueCanvas"></canvas>
                        </div>
                        <div style="display:flex; justify-content:space-between; align-items:center;">
                            <button class="btn btn-outline" style="font-size:12px; padding:6px 12px;" onclick="clearIssueCanvas()">Очистить холст</button>
                            <span style="font-size:11.5px; color:var(--text-tertiary);">Поставьте роспись стилусом или мышью</span>
                        </div>
                    </div>
                </div>

                <div class="card" style="margin-top:20px; display:flex; justify-content:space-between; align-items:center;">
                    <div>
                        <div style="font-weight:700;">Файл формата .tgl</div>
                        <div style="font-size:12.5px; color:var(--text-secondary);">Ключ будет сформирован с криптографическим идентификатором и метаданными сроков.</div>
                    </div>
                    <button class="btn btn-primary" style="padding:12px 24px;" onclick="saveAndDownloadTGL()">
                        <svg class="icon" viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
                        Сформировать и скачать .tgl
                    </button>
                </div>
            </div>

            <!-- 4. ПОДПИСКИ И СЕРВИСЫ -->
            <div id="page-subs" class="page">
                <h2 style="font-size:22px; font-weight:800; margin-bottom:6px;">Пакеты сервисов и подписок</h2>
                <div style="color:var(--text-secondary); font-size:13.5px; margin-bottom:24px;">Привязка партнерских программ Сбера непосредственно в файл ключа клиента.</div>

                <div class="card" style="margin-bottom:20px;">
                    <div class="file-zone" id="subDropZone">
                        <svg class="icon icon-lg" style="color:var(--text-secondary); margin-bottom:8px;" viewBox="0 0 24 24"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/></svg>
                        <div style="font-weight:700;" id="subFileStatus">Загрузите .tgl файл клиента</div>
                        <div style="font-size:12px; color:var(--text-tertiary); margin-top:4px;">Нажмите или перетащите файл для считывания подписок</div>
                        <input type="file" id="subFileInput" accept=".tgl,.std">
                    </div>
                </div>

                <div id="subsContainer" style="display:none;">
                    <div class="subs-grid" style="margin-bottom:24px;">
                        <label class="sub-card">
                            <input type="checkbox" id="checkSberPrime">
                            <div class="sub-icon-wrap">
                                <svg class="icon" viewBox="0 0 24 24"><path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/></svg>
                            </div>
                            <div>
                                <div style="font-weight:700; font-size:14px;">Сбер Прайм</div>
                                <div style="font-size:12px; color:var(--text-secondary); margin-top:3px;">Фильмы, музыка, кешбэк бонусами СберСпасибо и бесплатные переводы.</div>
                            </div>
                        </label>

                        <label class="sub-card">
                            <input type="checkbox" id="checkSberPlus">
                            <div class="sub-icon-wrap">
                                <svg class="icon" viewBox="0 0 24 24"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
                            </div>
                            <div>
                                <div style="font-weight:700; font-size:14px;">СберБизнес Плюс</div>
                                <div style="font-size:12px; color:var(--text-secondary); margin-top:3px;">Неограниченные платежки юрлицам и продлённый операционный день.</div>
                            </div>
                        </label>

                        <label class="sub-card">
                            <input type="checkbox" id="checkEDOPro">
                            <div class="sub-icon-wrap">
                                <svg class="icon" viewBox="0 0 24 24"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>
                            </div>
                            <div>
                                <div style="font-weight:700; font-size:14px;">TGL / Сбер Документооборот</div>
                                <div style="font-size:12px; color:var(--text-secondary); margin-top:3px;">Мгновенная юридическая верификация и облачный архив 5 лет.</div>
                            </div>
                        </label>
                    </div>

                    <button class="btn btn-primary" onclick="saveUpdatedSubsToFile()">
                        <svg class="icon" viewBox="0 0 24 24"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></svg>
                        Записать подписки и скачать обновленный файл
                    </button>
                </div>
            </div>

            <!-- 5. ПЕЧАТЬ ДОКУМЕНТОВ -->
            <div id="page-docs" class="page">
                <h2 style="font-size:22px; font-weight:800; margin-bottom:20px;">Печать и заверение документов</h2>

                <div class="grid-2" style="margin-bottom:20px;">
                    <div class="card">
                        <div class="card-title">1. Ключ Клиента (.tgl)</div>
                        <div class="file-zone" id="docClientDrop">
                            <svg class="icon icon-lg" viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
                            <div style="font-weight:700; margin-top:6px;" id="docClientText">Не загружен</div>
                            <input type="file" id="docClientFile" accept=".tgl,.std">
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-title">2. Ключ Сотрудника (.tgl)</div>
                        <div class="file-zone" id="docStaffDrop">
                            <svg class="icon icon-lg" viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/><polyline points="17 11 19 13 23 9"/></svg>
                            <div style="font-weight:700; margin-top:6px;" id="docStaffText">Использовать текущий профиль</div>
                            <input type="file" id="docStaffFile" accept=".tgl,.std">
                        </div>
                    </div>
                </div>

                <div class="card">
                    <div class="input-group">
                        <label>Выберите типовую форму документа:</label>
                        <select id="docTemplateSelect" class="input-control">
                            <option value="act">Акт приема-передачи ключа электронной подписи</option>
                            <option value="service">Договор банковского обслуживания и ЭДО</option>
                            <option value="prime">Соглашение о подключении опций программы СберПрайм</option>
                            <option value="nda">Соглашение о конфиденциальности персональных данных</option>
                        </select>
                    </div>

                    <button class="btn btn-primary" onclick="renderDocumentModal()">
                        <svg class="icon" viewBox="0 0 24 24"><polyline points="6 9 6 2 18 2 18 9"/><path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"/><rect x="6" y="14" width="12" height="8"/></svg>
                        Сформировать предпросмотр и напечатать
                    </button>
                </div>
            </div>

            <!-- 6. ПРОВЕРКА СРОКОВ И АННУЛИРОВАНИЕ -->
            <div id="page-verify" class="page">
                <h2 style="font-size:22px; font-weight:800; margin-bottom:6px;">Проверка легитимности и срока действия</h2>
                <div style="color:var(--text-secondary); font-size:13.5px; margin-bottom:24px;">Криптографический анализ даты, времени и статуса отзыва сертификата.</div>

                <div class="card">
                    <div class="file-zone" style="padding:40px;">
                        <svg class="icon" style="width:36px; height:36px; color:var(--sber-green); margin-bottom:10px;" viewBox="0 0 24 24"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg>
                        <div style="font-size:16px; font-weight:700;">Перетащите файл .tgl для проверки</div>
                        <div style="color:var(--text-secondary); font-size:12.5px; margin-top:4px;">Система мгновенно сверит штамп времени и статус</div>
                        <input type="file" id="verifyFileInput" accept=".tgl,.std">
                    </div>

                    <div id="verifyResultCard" style="display:none; margin-top:24px; border-top:1px solid var(--border-color); padding-top:24px;">
                        <div id="verifyStatusBadge" style="margin-bottom:16px;"></div>

                        <div class="grid-2" style="margin-bottom:20px;">
                            <div>
                                <div style="font-size:12px; color:var(--text-secondary);">ВЛАДЕЛЕЦ ПОДПИСИ:</div>
                                <div id="vFio" style="font-size:18px; font-weight:700;">-</div>
                            </div>
                            <div>
                                <div style="font-size:12px; color:var(--text-secondary);">ДЕЙСТВУЕТ ДО (ДАТА И ВРЕМЯ):</div>
                                <div id="vDate" style="font-size:18px; font-weight:700;">-</div>
                            </div>
                        </div>

                        <div style="margin-bottom:20px;">
                            <div style="font-size:12px; color:var(--text-secondary); margin-bottom:6px;">ПОДКЛЮЧЁННЫЕ ПОДПИСКИ:</div>
                            <div id="vSubs" style="font-weight:600; color:var(--sber-green);">Нет активных</div>
                        </div>

                        <div style="background:#fff; border:1px solid var(--border-color); border-radius:12px; padding:16px; text-align:center; margin-bottom:20px;">
                            <div style="font-size:11px; color:var(--text-tertiary); margin-bottom:8px;">ОТПЕЧАТОК ФАКСИМИЛЕ:</div>
                            <img id="vImg" style="height:90px; object-fit:contain;" src="">
                        </div>

                        <button class="btn btn-danger" onclick="revokeCurrentVerifiedKey()">
                            <svg class="icon" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><line x1="15" y1="9" x2="9" y2="15"/><line x1="9" y1="9" x2="15" y2="15"/></svg>
                            Аннулировать подпись (Отозвать сертификат)
                        </button>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <!-- ОКНО ПРЕДПРОСМОТРА ДОКУМЕНТА (А4) -->
    <div id="docOverlay">
        <div class="a4-sheet">
            <div class="a4-header" id="a4Title">ДОКУМЕНТ</div>
            <div class="a4-body" id="a4Content"></div>
            <div class="a4-sigs">
                <div class="a4-sig-block">
                    <div style="font-weight:bold; font-size:13px; margin-bottom:4px;">СОТРУДНИК БАНКА</div>
                    <img id="a4SigStaff" class="a4-sig-img" src="">
                    <div style="font-size:13px;" id="a4StaffName">ФИО Сотрудника</div>
                </div>
                <div class="a4-sig-block">
                    <div style="font-weight:bold; font-size:13px; margin-bottom:4px;">КЛИЕНТ</div>
                    <img id="a4SigClient" class="a4-sig-img" src="">
                    <div style="font-size:13px;" id="a4ClientName">ФИО Клиента</div>
                </div>
            </div>
        </div>
        <div class="doc-bar">
            <button class="btn btn-outline" onclick="document.getElementById('docOverlay').style.display='none'">Закрыть</button>
            <button class="btn btn-primary" onclick="window.print()">
                <svg class="icon" viewBox="0 0 24 24"><polyline points="6 9 6 2 18 2 18 9"/><path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"/><rect x="6" y="14" width="12" height="8"/></svg>
                Распечатать документ
            </button>
        </div>
    </div>

    <script>
        // ИНИЦИАЛИЗАЦИЯ И ДАННЫЕ ОПЕРАТОРА
        let currentEmployee = {
            fio: localStorage.getItem('sber_staff_fio') || "Бондарева Анастасия Александровна",
            role: localStorage.getItem('sber_staff_role') || "Ведущий специалист ЭДО, Воронежское отд. №9013"
        };

        window.addEventListener('DOMContentLoaded', () => {
            if(!localStorage.getItem('sber_staff_fio')) {
                document.getElementById('authModal').style.display = 'flex';
            } else {
                document.getElementById('authModal').style.display = 'none';
                applyEmployeeProfile();
            }

            // Установка даты по умолчанию (+1 год вперед)
            const date = new Date();
            date.setFullYear(date.getFullYear() + 1);
            date.setMinutes(date.getMinutes() - date.getTimezoneOffset());
            document.getElementById('newExpiry').value = date.toISOString().slice(0,16);

            initIssueCanvas();
        });

        function openAuthModal() {
            document.getElementById('authFioInput').value = currentEmployee.fio;
            document.getElementById('authRoleInput').value = currentEmployee.role;
            document.getElementById('authModal').style.display = 'flex';
        }

        function saveEmployeeAuth() {
            const fio = document.getElementById('authFioInput').value.trim();
            const role = document.getElementById('authRoleInput').value.trim();
            if(!fio) return alert('Введите ФИО сотрудника');

            currentEmployee.fio = fio;
            currentEmployee.role = role;
            localStorage.setItem('sber_staff_fio', fio);
            localStorage.setItem('sber_staff_role', role);

            applyEmployeeProfile();
            document.getElementById('authModal').style.display = 'none';
        }

        function applyEmployeeProfile() {
            const parts = currentEmployee.fio.split(' ');
            let short = parts[0];
            let initials = parts[0][0] || 'С';
            if(parts[1]) { short += ' ' + parts[1][0] + '.'; initials += parts[1][0]; }
            if(parts[2]) { short += ' ' + parts[2][0] + '.'; }

            document.getElementById('headerFioDisplay').innerText = short;
            document.getElementById('headerAvatar').innerText = initials.toUpperCase();
            document.getElementById('headerRoleDisplay').innerText = currentEmployee.role.split(',')[0];
        }

        function syncApp() {
            const btn = document.querySelector('.sync-btn');
            btn.style.transform = 'rotate(360deg)';
            setTimeout(() => {
                btn.style.transform = 'none';
                alert('Данные реестра синхронизированы с сервером.');
            }, 400);
        }

        // НАВИГАЦИЯ
        function switchNav(pageId, element) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            if(element) element.classList.add('active');
            if(pageId === 'page-issue') resizeIssueCanvas();
        }

        // РИСОВАНИЕ ПОДПИСИ (ХОЛСТ)
        const canvas = document.getElementById('issueCanvas');
        const ctx = canvas.getContext('2d');
        let drawing = false, hasDrawn = false;

        function initIssueCanvas() {
            ctx.strokeStyle = "#0d1b2a";
            ctx.lineWidth = 2.5;
            ctx.lineCap = "round";
            ctx.lineJoin = "round";

            const getP = (e) => {
                const r = canvas.getBoundingClientRect();
                const x = e.touches ? e.touches[0].clientX : e.clientX;
                const y = e.touches ? e.touches[0].clientY : e.clientY;
                return { x: x - r.left, y: y - r.top };
            };

            const start = (e) => { e.preventDefault(); drawing = true; hasDrawn = true; const p = getP(e); ctx.beginPath(); ctx.moveTo(p.x, p.y); };
            const move = (e) => { if(!drawing) return; e.preventDefault(); const p = getP(e); ctx.lineTo(p.x, p.y); ctx.stroke(); };
            const stop = () => { drawing = false; };

            canvas.addEventListener('mousedown', start); canvas.addEventListener('mousemove', move); window.addEventListener('mouseup', stop);
            canvas.addEventListener('touchstart', start, {passive:false}); canvas.addEventListener('touchmove', move, {passive:false}); window.addEventListener('touchend', stop);
        }

        function resizeIssueCanvas() {
            if(canvas.parentElement.clientWidth > 0) {
                canvas.width = canvas.parentElement.clientWidth;
                canvas.height = 180;
                ctx.fillStyle = "#ffffff";
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                ctx.strokeStyle = "#0d1b2a";
                ctx.lineWidth = 2.5;
                ctx.lineCap = "round";
            }
        }
        window.addEventListener('resize', resizeIssueCanvas);

        function clearIssueCanvas() {
            ctx.fillStyle = "#ffffff";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            hasDrawn = false;
        }

        // ВЫПУСК ЭЦП В ФАЙЛ
        function saveAndDownloadTGL() {
            const fio = document.getElementById('newFio').value.trim();
            const inn = document.getElementById('newInn').value.trim();
            const expiry = document.getElementById('newExpiry').value;

            if(!fio || !expiry || !hasDrawn) {
                return alert('Пожалуйста, заполните ФИО, установите срок окончания и поставьте подпись на холсте!');
            }

            const data = {
                type: "tgl",
                version: "2.0",
                fio: fio,
                inn: inn || "Не указан",
                signature: canvas.toDataURL("image/png"),
                validUntil: new Date(expiry).toISOString(),
                subscriptions: [],
                issuedBy: currentEmployee.fio,
                issuedAt: new Date().toISOString()
            };

            const blob = new Blob([JSON.stringify(data, null, 2)], { type: "application/octet-stream" });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = `Ключ_${fio.split(' ')[0]}.tgl`;
            a.click();

            // Добавляем запись в таблицу истории
            addOperationRow(fio, "Выпуск усиленного сертификата ЭЦП", "0,00 ₽");
            alert(`✅ ЭЦП для ${fio} успешно создана и скачана!`);
        }

        function addOperationRow(name, desc, sum) {
            const tbody = document.getElementById('operationsList');
            const num = Math.floor(Math.random() * 899 + 100);
            const tr = document.createElement('tr');
            tr.innerHTML = `
                <td>
                    <div style="font-weight:700;">№ ${num}</div>
                    <div style="font-size:11.5px; color:var(--text-tertiary);">Только что</div>
                </td>
                <td>
                    <div style="font-weight:600;">${name.toUpperCase()}</div>
                    <div style="font-size:11.5px; color:var(--text-tertiary);">Сотрудник: ${currentEmployee.fio.split(' ')[0]}</div>
                </td>
                <td>${desc}</td>
                <td style="font-weight:700;">${sum}</td>
                <td><span class="status-badge status-success"><span class="status-dot"></span>Исполнено</span></td>
                <td style="text-align:right;">
                    <button class="btn btn-outline" style="padding:4px 8px;" onclick="window.print()"><svg class="icon icon-sm" viewBox="0 0 24 24"><polyline points="6 9 6 2 18 2 18 9"/><path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"/><rect x="6" y="14" width="12" height="8"/></svg></button>
                </td>
            `;
            tbody.insertBefore(tr, tbody.firstChild);
        }

        // УПРАВЛЕНИЕ ПОДПИСКАМИ
        let activeSubFileData = null;
        document.getElementById('subFileInput').addEventListener('change', function(e) {
            const file = e.target.files[0];
            if(!file) return;
            const r = new FileReader();
            r.onload = ev => {
                try {
                    activeSubFileData = JSON.parse(ev.target.result);
                    document.getElementById('subFileStatus').innerText = "✅ " + activeSubFileData.fio;
                    document.getElementById('subDropZone').classList.add('loaded');
                    document.getElementById('subsContainer').style.display = 'block';

                    const subs = activeSubFileData.subscriptions || [];
                    document.getElementById('checkSberPrime').checked = subs.includes("Сбер Прайм");
                    document.getElementById('checkSberPlus').checked = subs.includes("СберБизнес Плюс");
                    document.getElementById('checkEDOPro').checked = subs.includes("TGL / Сбер Документооборот");
                } catch(err) { alert('Неверный формат файла .tgl'); }
            };
            r.readAsText(file);
        });

        function saveUpdatedSubsToFile() {
            if(!activeSubFileData) return;
            const newSubs = [];
            if(document.getElementById('checkSberPrime').checked) newSubs.push("Сбер Прайм");
            if(document.getElementById('checkSberPlus').checked) newSubs.push("СберБизнес Плюс");
            if(document.getElementById('checkEDOPro').checked) newSubs.push("TGL / Сбер Документооборот");

            activeSubFileData.subscriptions = newSubs;
            const blob = new Blob([JSON.stringify(activeSubFileData, null, 2)], { type: "application/octet-stream" });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = `Ключ_Подписки_${activeSubFileData.fio.split(' ')[0]}.tgl`;
            a.click();

            addOperationRow(activeSubFileData.fio, "Обновление партнерских подписок", "390,00 ₽");
            alert('Пакет подписок обновлен! Файл сохранен.');
        }

        // ПЕЧАТЬ ДОКУМЕНТОВ
        let docClientKey = null;
        let docStaffKey = null;

        document.getElementById('docClientFile').addEventListener('change', (e) => {
            const f = e.target.files[0]; if(!f) return;
            const r = new FileReader();
            r.onload = ev => {
                try {
                    docClientKey = JSON.parse(ev.target.result);
                    document.getElementById('docClientText').innerText = "✅ " + docClientKey.fio;
                    document.getElementById('docClientDrop').classList.add('loaded');
                } catch(e) { alert('Ошибка ключа клиента'); }
            }; r.readAsText(f);
        });

        document.getElementById('docStaffFile').addEventListener('change', (e) => {
            const f = e.target.files[0]; if(!f) return;
            const r = new FileReader();
            r.onload = ev => {
                try {
                    docStaffKey = JSON.parse(ev.target.result);
                    document.getElementById('docStaffText').innerText = "✅ " + docStaffKey.fio;
                    document.getElementById('docStaffDrop').classList.add('loaded');
                } catch(e) { alert('Ошибка ключа сотрудника'); }
            }; r.readAsText(f);
        });

        function renderDocumentModal() {
            if(!docClientKey) return alert('Пожалуйста, загрузите файл ключа клиента!');
            const tpl = document.getElementById('docTemplateSelect').value;

            let title = "АКТ ПРИЕМА-ПЕРЕДАЧИ КЛЮЧА ЭЦП";
            let content = `ПАО «Сбербанк» в лице уполномоченного сотрудника ${currentEmployee.fio} с одной стороны, и клиент ${docClientKey.fio} с другой стороны, подтверждают успешный выпуск и передачу сертификата усиленной квалифицированной электронной подписи в системе СберБизнес.\n\nКлиент подтверждает отсутствие претензий и ознакомлен с регламентом использования закрытого ключа.`;

            if(tpl === 'prime') {
                title = "СОГЛАШЕНИЕ О ПОДКЛЮЧЕНИИ ОПЦИЙ СБЕРПРАЙМ";
                content = `Настоящим подтверждается активация комплексного обслуживания СберПрайм для клиента ${docClientKey.fio}.\nСервисы: Доставка, Фильмы, Музыка, Кешбэк бонусами "Спасибо".\nСотрудник отделения: ${currentEmployee.fio}.`;
            } else if(tpl === 'service') {
                title = "ДОГОВОР БАНКОВСКОГО ОБСЛУЖИВАНИЯ И ЭДО";
                content = `Клиент ${docClientKey.fio} присоединяется к правилам комплексного банковского обслуживания юридических лиц и индивидуальных предпринимателей в системе СберБизнес.`;
            }

            document.getElementById('a4Title').innerText = title;
            document.getElementById('a4Content').innerText = content;
            document.getElementById('a4StaffName').innerText = currentEmployee.fio;
            document.getElementById('a4ClientName').innerText = docClientKey.fio;

            // Факсимиле клиента
            document.getElementById('a4SigClient').src = docClientKey.signature;

            // Факсимиле сотрудника (если загружен файл, иначе холст или пустое поле)
            if(docStaffKey && docStaffKey.signature) {
                document.getElementById('a4SigStaff').src = docStaffKey.signature;
            } else {
                document.getElementById('a4SigStaff').src = canvas.toDataURL();
            }

            document.getElementById('docOverlay').style.display = 'block';
        }

        // ПРОВЕРКА И АННУЛИРОВАНИЕ
        let verifiedKeyData = null;
        document.getElementById('verifyFileInput').addEventListener('change', function(e) {
            const f = e.target.files[0]; if(!f) return;
            const r = new FileReader();
            r.onload = ev => {
                try {
                    verifiedKeyData = JSON.parse(ev.target.result);
                    document.getElementById('verifyResultCard').style.display = 'block';
                    document.getElementById('vFio').innerText = verifiedKeyData.fio;
                    document.getElementById('vImg').src = verifiedKeyData.signature;

                    const subs = verifiedKeyData.subscriptions || [];
                    document.getElementById('vSubs').innerText = subs.length ? subs.join(', ') : 'Нет активных пакетов';

                    const badge = document.getElementById('verifyStatusBadge');
                    if(verifiedKeyData.validUntil) {
                        const exp = new Date(verifiedKeyData.validUntil);
                        const now = new Date();
                        const str = exp.toLocaleDateString('ru-RU') + " " + exp.toLocaleTimeString('ru-RU', {hour:'2-digit', minute:'2-digit'});
                        document.getElementById('vDate').innerText = str;

                        if(now > exp) {
                            badge.innerHTML = `<span class="status-badge status-danger" style="font-size:16px;"><span class="status-dot"></span>СРОК ДЕЙСТВИЯ ПОДПИСИ ИСТЁК</span>`;
                        } else {
                            badge.innerHTML = `<span class="status-badge status-success" style="font-size:16px;"><span class="status-dot"></span>ПОДПИСЬ ДЕЙСТВИТЕЛЬНА</span>`;
                        }
                    } else {
                        document.getElementById('vDate').innerText = "Бессрочно (Архивная)";
                        badge.innerHTML = `<span class="status-badge status-success" style="font-size:16px;"><span class="status-dot"></span>ДЕЙСТВИТЕЛЬНА</span>`;
                    }
                } catch(err) { alert('Не удалось прочитать файл'); }
            }; r.readAsText(f);
        });

        function revokeCurrentVerifiedKey() {
            if(!verifiedKeyData) return;
            if(!confirm(`Вы действительно хотите отозвать подпись клиента ${verifiedKeyData.fio}?`)) return;

            // Переводим дату в прошлое
            const yesterday = new Date();
            yesterday.setDate(yesterday.getDate() - 1);
            verifiedKeyData.validUntil = yesterday.toISOString();

            const blob = new Blob([JSON.stringify(verifiedKeyData, null, 2)], { type: "application/octet-stream" });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = `АННУЛИРОВАН_${verifiedKeyData.fio.split(' ')[0]}.tgl`;
            a.click();

            addOperationRow(verifiedKeyData.fio, "Отзыв и аннулирование сертификата ЭЦП", "0,00 ₽");
            alert('🚫 Сертификат аннулирован. Файл с отметкой об истечении скачан.');
            document.getElementById('verifyResultCard').style.display = 'none';
        }
    </script>
</body>
</html>
