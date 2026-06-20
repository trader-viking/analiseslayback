<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Analisador Operacional v7.3.1</title>
<meta name="theme-color" content="#0a0e1a">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&family=JetBrains+Mono:wght@500;700;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
:root{--bg:#0a0e1a;--bg-elevated:#0f1524;--surface:#151c2e;--surface-2:#1a2238;--surface-3:#202942;--border:#1f2940;--border-strong:#2d3a5c;--text-primary:#f8fafc;--text-secondary:#cbd5e1;--text-tertiary:#94a3b8;--text-disabled:#64748b;--brand:#3b82f6;--brand-2:#06b6d4;--accent:#8b5cf6;--success:#10b981;--success-bg:rgba(16,185,129,.1);--success-border:rgba(16,185,129,.35);--warning:#f59e0b;--warning-bg:rgba(245,158,11,.1);--warning-border:rgba(245,158,11,.35);--danger:#ef4444;--danger-bg:rgba(239,68,68,.1);--danger-border:rgba(239,68,68,.35);--gold:#fbbf24;--gold-bg:rgba(251,191,36,.08);--gold-border:rgba(251,191,36,.45);--lime:#84cc16;--fs-2xs:10px;--fs-xs:11px;--fs-sm:13px;--fs-base:15px;--fs-md:16px;--fs-lg:18px;--fs-xl:22px;--fs-2xl:28px;--fs-3xl:36px;--sp-1:4px;--sp-2:8px;--sp-3:12px;--sp-4:16px;--sp-5:20px;--sp-6:24px;--sp-8:32px;--sp-10:40px;--sp-12:48px;--r-sm:6px;--r-md:10px;--r-lg:14px;--r-xl:18px;--sh-sm:0 1px 2px rgba(0,0,0,.3);--sh-md:0 4px 12px rgba(0,0,0,.4);--sh-lg:0 8px 24px rgba(0,0,0,.5);--sh-xl:0 16px 48px rgba(0,0,0,.6);--t-fast:.15s ease;--t-base:.25s cubic-bezier(.4,0,.2,1)}
*{box-sizing:border-box;margin:0;padding:0;-webkit-font-smoothing:antialiased}
html,body{background:var(--bg);color:var(--text-primary);font-family:'Inter',system-ui,sans-serif;min-height:100vh;font-size:var(--fs-base);line-height:1.5}
body{background:radial-gradient(ellipse at top,rgba(59,130,246,.06) 0%,transparent 50%),var(--bg)}
.num{font-family:'JetBrains Mono',monospace}
button,input,select,textarea{font-family:inherit}
.app{max-width:1280px;margin:0 auto;padding:var(--sp-5)}
.hdr{display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:var(--sp-4);padding:var(--sp-5) var(--sp-6);background:linear-gradient(135deg,var(--surface),var(--bg-elevated));border:1px solid var(--border);border-radius:var(--r-xl);margin-bottom:var(--sp-5);box-shadow:var(--sh-md)}
.hdr-brand{display:flex;align-items:center;gap:var(--sp-3)}
.hdr-logo{width:44px;height:44px;border-radius:var(--r-md);background:linear-gradient(135deg,var(--brand),var(--accent));display:flex;align-items:center;justify-content:center;font-weight:800;font-size:var(--fs-lg);color:#fff;box-shadow:0 4px 16px rgba(59,130,246,.4);overflow:hidden}
.hdr-logo img{width:100%;height:100%;object-fit:cover}
.hdr h1{font-size:var(--fs-xl);font-weight:800;letter-spacing:-.5px;background:linear-gradient(90deg,#fff,#94a3b8);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.hdr .sub{font-size:var(--fs-xs);color:var(--text-tertiary);margin-top:2px}
.hdr-pill{font-family:'JetBrains Mono';font-size:var(--fs-xs);color:var(--brand-2);background:rgba(6,182,212,.08);padding:var(--sp-2) var(--sp-3);border-radius:var(--r-md);border:1px solid rgba(6,182,212,.3)}
.upload{background:linear-gradient(135deg,var(--surface),var(--bg-elevated));border:2px dashed var(--border-strong);border-radius:var(--r-xl);padding:var(--sp-8) var(--sp-6);text-align:center;margin-bottom:var(--sp-5);transition:var(--t-base)}
.upload.dragover{border-color:var(--brand);transform:scale(1.005)}
.upload h3{font-size:var(--fs-lg);margin-bottom:var(--sp-2);font-weight:700}
.upload p{color:var(--text-tertiary);font-size:var(--fs-sm);margin-bottom:var(--sp-4)}
.upload-actions{display:flex;justify-content:center;gap:var(--sp-2);flex-wrap:wrap;margin-bottom:var(--sp-4)}
.btn{display:inline-flex;align-items:center;gap:var(--sp-2);background:var(--brand);color:#fff;border:none;padding:var(--sp-3) var(--sp-5);border-radius:var(--r-md);font-weight:600;cursor:pointer;transition:var(--t-fast);font-size:var(--fs-sm);box-shadow:0 2px 8px rgba(59,130,246,.2)}
.btn:hover:not(:disabled){filter:brightness(1.15);transform:translateY(-1px)}
.btn:disabled{opacity:.4;cursor:not-allowed;transform:none}
.btn.alt{background:var(--surface-2);color:var(--text-primary);box-shadow:none;border:1px solid var(--border-strong)}
.btn.alt:hover:not(:disabled){background:var(--surface-3)}
.btn.suc{background:linear-gradient(135deg,var(--success),#059669)}
.btn.dng{background:linear-gradient(135deg,var(--danger),#dc2626)}
.btn.inf{background:linear-gradient(135deg,var(--brand-2),#0891b2)}
.btn.gld{background:linear-gradient(135deg,var(--gold),#f59e0b);color:#1a1000}
.btn.acc{background:linear-gradient(135deg,var(--accent),#7c3aed)}
.btn.sm{padding:var(--sp-2) var(--sp-3);font-size:var(--fs-xs)}
.fl{list-style:none;margin-top:var(--sp-3);text-align:left;max-width:640px;margin-left:auto;margin-right:auto}
.fl li{background:var(--surface-2);padding:var(--sp-3) var(--sp-4);border-radius:var(--r-md);margin-bottom:var(--sp-2);font-size:var(--fs-sm);display:flex;justify-content:space-between;align-items:center;border:1px solid var(--border)}
.fl li .rm{color:var(--danger);cursor:pointer;font-weight:700;padding:2px 8px;border-radius:var(--r-sm)}
.tools{display:flex;flex-wrap:wrap;gap:var(--sp-3);align-items:center;background:var(--surface);padding:var(--sp-4);border-radius:var(--r-lg);border:1px solid var(--border);margin-bottom:var(--sp-4)}
.tools-info{display:flex;gap:var(--sp-3);align-items:center;flex:1;flex-wrap:wrap}
.tools-info .badge{padding:var(--sp-1) var(--sp-3);border-radius:999px;font-size:var(--fs-xs);font-weight:700;font-family:'JetBrains Mono';display:inline-flex;align-items:center;gap:4px}
.tools-info .badge.suc{background:var(--success-bg);color:var(--success);border:1px solid var(--success-border)}
.tools-info .badge.warn{background:var(--warning-bg);color:var(--warning);border:1px solid var(--warning-border)}
.tools-info .badge.gld{background:var(--gold-bg);color:var(--gold);border:1px solid var(--gold-border)}
.tools-info .badge.dng{background:var(--danger-bg);color:var(--danger);border:1px solid var(--danger-border)}
.match{background:linear-gradient(135deg,var(--surface),var(--bg-elevated));border:1px solid var(--border);border-radius:var(--r-xl);margin-bottom:var(--sp-5);overflow:hidden;box-shadow:var(--sh-md);transition:var(--t-base)}
.match:hover{border-color:var(--border-strong)}
.m-head{padding:var(--sp-5) var(--sp-6);background:linear-gradient(135deg,var(--surface-2),var(--surface));display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:var(--sp-4);cursor:pointer}
.m-head:hover{background:linear-gradient(135deg,var(--surface-3),var(--surface-2))}
.m-title{display:flex;flex-direction:column;gap:8px;flex:1;min-width:0}
.m-teams{font-size:var(--fs-xl);font-weight:800;letter-spacing:-.5px;display:flex;align-items:center;gap:var(--sp-2);flex-wrap:wrap}
.m-vs{color:var(--text-tertiary);font-weight:400;font-size:var(--fs-md);margin:0 var(--sp-2)}
.fmt-badge{display:inline-flex;background:rgba(6,182,212,.1);border:1px solid rgba(6,182,212,.3);color:var(--brand-2);padding:3px 8px;border-radius:var(--r-sm);font-size:var(--fs-2xs);font-family:'JetBrains Mono';font-weight:700}
.m-chips{display:flex;flex-wrap:wrap;gap:var(--sp-2);align-items:center;margin-top:2px}
.chip{display:inline-flex;align-items:center;gap:6px;padding:6px 12px;border-radius:999px;font-size:var(--fs-xs);font-weight:700;font-family:'JetBrains Mono';border:1px solid;cursor:pointer;transition:var(--t-fast);user-select:none}
.chip:hover{filter:brightness(1.2);transform:translateY(-1px)}
.chip .ic{font-size:var(--fs-sm)}
.chip.date{background:rgba(139,92,246,.12);color:#c4b5fd;border-color:rgba(139,92,246,.35)}
.chip.time{background:rgba(6,182,212,.12);color:#67e8f9;border-color:rgba(6,182,212,.4)}
.chip.time.live{background:linear-gradient(135deg,rgba(239,68,68,.2),rgba(245,158,11,.15));color:#fca5a5;border-color:rgba(239,68,68,.45);animation:livePulse 2s ease-in-out infinite}
.chip.time.soon{background:rgba(245,158,11,.15);color:#fcd34d;border-color:rgba(245,158,11,.4)}
.chip.comp{background:rgba(251,191,36,.1);color:var(--gold);border-color:rgba(251,191,36,.35)}
.chip.round{background:rgba(148,163,184,.1);color:var(--text-secondary);border-color:rgba(148,163,184,.3)}
.chip .edit-ic{font-size:9px;opacity:.5;margin-left:4px}
.chip:hover .edit-ic{opacity:1}
@keyframes livePulse{0%,100%{box-shadow:0 0 0 rgba(239,68,68,0)}50%{box-shadow:0 0 12px rgba(239,68,68,.5)}}
.chip-edit{display:inline-flex;align-items:center;gap:6px;padding:4px 8px;border-radius:999px;border:2px solid var(--brand);background:var(--surface-3)}
.chip-edit input{background:var(--bg-elevated);border:1px solid var(--border);color:var(--text-primary);padding:4px 8px;border-radius:var(--r-sm);font-family:'JetBrains Mono';font-size:var(--fs-xs);font-weight:700;width:100px;text-align:center}
.chip-edit button{background:var(--success);color:#fff;border:none;padding:4px 8px;border-radius:var(--r-sm);font-size:10px;font-weight:700;cursor:pointer}
.chip-edit button.cancel{background:var(--text-disabled)}
.m-count{display:flex;align-items:center;gap:var(--sp-3)}
.m-count .pill{padding:var(--sp-2) var(--sp-4);border-radius:999px;font-weight:800;font-family:'JetBrains Mono';display:inline-flex;align-items:center;gap:6px;font-size:var(--fs-sm)}
.m-count .pill.suc{background:var(--success-bg);color:var(--success);border:1px solid var(--success-border)}
.m-count .pill.suc .n{font-size:var(--fs-lg)}
.m-count .pill.warn{background:var(--warning-bg);color:var(--warning);border:1px solid var(--warning-border)}
.m-count .chev{color:var(--text-tertiary);font-size:var(--fs-lg);transition:var(--t-fast)}
.match.open .m-count .chev{transform:rotate(180deg)}
.pois-banner{background:var(--surface-2);padding:var(--sp-4) var(--sp-6);border-top:1px solid var(--border);border-bottom:1px solid var(--border);display:none;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:var(--sp-3)}
.match.open .pois-banner{display:flex}
.pois-banner-l{display:flex;gap:var(--sp-4);align-items:center;flex-wrap:wrap}
.pois-stat{display:flex;flex-direction:column;gap:2px}
.pois-stat .lbl{font-size:var(--fs-2xs);color:var(--text-tertiary);text-transform:uppercase;letter-spacing:.5px;font-weight:600}
.pois-stat .v{font-family:'JetBrains Mono';font-size:var(--fs-lg);font-weight:800}
.pois-stat.h .v{color:var(--success)}
.pois-stat.d .v{color:var(--warning)}
.pois-stat.a .v{color:var(--brand-2)}
.pois-banner-r{display:flex;gap:var(--sp-2);flex-wrap:wrap}
.pois-banner-r .ts{background:var(--surface-3);padding:6px 10px;border-radius:var(--r-sm);font-family:'JetBrains Mono';font-size:var(--fs-xs);font-weight:600;border:1px solid var(--border);color:var(--text-secondary)}
.pois-banner-r .ts b{color:var(--gold);font-weight:800;margin-left:6px}
.match-odds{display:none;padding:var(--sp-5) var(--sp-6);background:linear-gradient(135deg,rgba(251,191,36,.04),rgba(245,158,11,.02));border-bottom:1px solid var(--border)}
.match.open .match-odds{display:block}
.mo-head{display:flex;justify-content:space-between;align-items:center;gap:var(--sp-3);flex-wrap:wrap;margin-bottom:var(--sp-4)}
.mo-head-title h3{font-size:var(--fs-md);font-weight:800;color:var(--gold);display:flex;align-items:center;gap:6px}
.mo-head-actions{display:flex;gap:var(--sp-2);flex-wrap:wrap;align-items:center}
.mo-fill-stats{display:flex;gap:var(--sp-2);font-size:var(--fs-xs);font-family:'JetBrains Mono';font-weight:700;align-items:center;flex-wrap:wrap}
.mo-fill-stats .stat{padding:4px 10px;border-radius:999px}
.mo-fill-stats .stat.filled{background:var(--success-bg);color:var(--success);border:1px solid var(--success-border)}
.mo-fill-stats .stat.empty{background:var(--surface-3);color:var(--text-tertiary);border:1px solid var(--border)}
.mo-section{margin-bottom:var(--sp-4)}
.mo-section:last-child{margin-bottom:0}
.mo-section-h{font-size:10px;color:var(--text-tertiary);text-transform:uppercase;letter-spacing:.6px;font-weight:700;margin-bottom:var(--sp-2);display:flex;align-items:center;gap:6px;padding-left:2px}
.mo-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(150px,1fr));gap:var(--sp-2)}
.mo-cell{background:var(--surface-3);border:1px solid var(--border);border-radius:var(--r-md);padding:var(--sp-2) var(--sp-3);transition:var(--t-fast);position:relative}
.mo-cell:focus-within{border-color:var(--brand);background:var(--bg-elevated)}
.mo-cell.has-val{border-color:var(--brand-2)}
.mo-cell label{display:flex;align-items:center;justify-content:space-between;font-size:10px;color:var(--text-tertiary);text-transform:uppercase;letter-spacing:.4px;font-weight:700;margin-bottom:3px;gap:6px}
.mo-cell label .imp{color:var(--gold);font-family:'JetBrains Mono';font-weight:800;font-size:9px}
.mo-cell input{width:100%;background:transparent;border:none;color:var(--text-primary);font-family:'JetBrains Mono';font-size:var(--fs-md);font-weight:800;text-align:center;letter-spacing:-.3px}
.mo-cell input:focus{outline:none}
.mo-cell input::-webkit-outer-spin-button,.mo-cell input::-webkit-inner-spin-button{-webkit-appearance:none;margin:0}
.mo-cell input[type=number]{-moz-appearance:textfield}
.mo-cell input::placeholder{color:var(--text-disabled);font-weight:500;font-size:var(--fs-sm)}
.mo-cell .ev-badge{position:absolute;top:-6px;right:6px;font-size:8px;font-weight:800;padding:2px 5px;border-radius:var(--r-sm);font-family:'JetBrains Mono';letter-spacing:.3px}
.mo-cell .ev-badge.pos{background:var(--success);color:#fff}
.mo-cell .ev-badge.neg{background:var(--danger);color:#fff}
.mo-cell.has-val.ev-pos{box-shadow:0 0 0 1px var(--success);background:linear-gradient(180deg,var(--surface-3),rgba(16,185,129,.05))}
.mo-cell.has-val.ev-neg{opacity:.75}
.mo-cell.flash{animation:flashCell .6s ease-out}
@keyframes flashCell{0%{background:rgba(132,204,22,.4)}100%{background:var(--surface-3)}}
.mo-actions{display:flex;gap:var(--sp-2);margin-top:var(--sp-4);flex-wrap:wrap}
.entries{display:none;padding:var(--sp-5) var(--sp-6);flex-direction:column;gap:var(--sp-3)}
.match.open .entries{display:flex}
.entry{background:var(--surface-2);border:1px solid var(--border);border-radius:var(--r-lg);overflow:hidden;transition:var(--t-base);position:relative}
.entry:hover{border-color:var(--border-strong)}
.entry.hv{border:2px solid transparent;background:linear-gradient(var(--surface-2),var(--surface-2)) padding-box,linear-gradient(90deg,var(--gold),#f59e0b,var(--gold),#f59e0b) border-box;background-size:200% 100%;animation:shineGold 3s linear infinite}
@keyframes shineGold{0%{background-position:0% 0,0% 0}100%{background-position:0% 0,200% 0}}
.entry.s-no{opacity:.55}
.entry.s-no:hover{opacity:.8}
.entry.s-wait{border-color:var(--warning)}
.entry.s-execute,.entry.s-hv{box-shadow:0 0 14px rgba(16,185,129,.15)}
.entry .rank{position:absolute;top:8px;left:8px;width:24px;height:24px;border-radius:50%;background:var(--brand);color:#fff;display:flex;align-items:center;justify-content:center;font-family:'JetBrains Mono';font-weight:900;font-size:11px;box-shadow:var(--sh-sm);z-index:1}
.entry .rank.r1{background:linear-gradient(135deg,var(--gold),#f59e0b);color:#1a1000;box-shadow:0 0 12px rgba(251,191,36,.5)}
.entry .rank.r2{background:linear-gradient(135deg,#c0c0c0,#8b8b8b);color:#1a1a1a}
.entry .rank.r3{background:linear-gradient(135deg,#cd7f32,#8b4513);color:#fff}
.entry-head{padding:var(--sp-4) var(--sp-5);padding-left:42px;cursor:pointer;display:flex;justify-content:space-between;align-items:center;gap:var(--sp-4);transition:var(--t-fast)}
.entry-head:hover{background:var(--surface-3)}
.entry-l{display:flex;align-items:center;gap:var(--sp-4);flex:1;min-width:0}
.entry-num{display:flex;align-items:center;justify-content:center;width:48px;height:48px;flex-shrink:0;background:linear-gradient(135deg,var(--brand),var(--accent));color:#fff;border-radius:var(--r-md);font-family:'JetBrains Mono';font-weight:800;font-size:var(--fs-md);box-shadow:var(--sh-sm)}
.entry.hv .entry-num{background:linear-gradient(135deg,var(--gold),#f59e0b);color:#1a1000;box-shadow:0 0 16px rgba(251,191,36,.4)}
.entry-text{min-width:0;flex:1}
.entry-name{font-weight:700;font-size:var(--fs-md);letter-spacing:-.2px;display:flex;align-items:center;gap:var(--sp-2);flex-wrap:wrap}
.entry-name .star{color:var(--gold);font-size:var(--fs-sm)}
.entry-mkt{font-size:var(--fs-xs);color:var(--text-tertiary);margin-top:2px;display:flex;align-items:center;gap:var(--sp-2);flex-wrap:wrap}
.entry-mkt .tag{padding:2px 7px;border-radius:var(--r-sm);background:rgba(6,182,212,.1);color:var(--brand-2);font-weight:600;border:1px solid rgba(6,182,212,.25);text-transform:uppercase;font-family:'JetBrains Mono';font-size:9px;letter-spacing:.4px}
.entry-mkt .tag.back{background:var(--success-bg);color:var(--success);border-color:var(--success-border)}
.entry-mkt .tag.lay{background:rgba(236,72,153,.1);color:#f9a8d4;border-color:rgba(236,72,153,.3)}
.entry-mkt .prob{padding:2px 7px;border-radius:var(--r-sm);font-weight:700;font-family:'JetBrains Mono';font-size:10px}
.entry-mkt .prob.high{background:rgba(132,204,22,.12);color:var(--lime);border:1px solid rgba(132,204,22,.3)}
.entry-mkt .prob.mid{background:var(--warning-bg);color:var(--warning);border:1px solid var(--warning-border)}
.entry-mkt .prob.low{background:var(--danger-bg);color:var(--danger);border:1px solid var(--danger-border)}
.entry-r{display:flex;align-items:center;gap:var(--sp-3);flex-shrink:0}
.entry-score{display:flex;flex-direction:column;align-items:flex-end;font-family:'JetBrains Mono'}
.entry-score .n{font-size:var(--fs-2xl);font-weight:900;line-height:1;letter-spacing:-1px;color:var(--success)}
.entry-score .pct{font-size:var(--fs-sm);color:var(--text-tertiary);font-weight:700}
.entry-chev{color:var(--text-tertiary);transition:var(--t-fast);font-size:var(--fs-lg)}
.entry.open .entry-chev{transform:rotate(180deg);color:var(--success)}
.entry-head-status{margin-right:var(--sp-2);padding:4px 10px;border-radius:999px;font-size:10px;font-weight:800;font-family:'JetBrains Mono';white-space:nowrap;letter-spacing:.3px}
.ehs-execute{background:var(--success-bg);color:var(--success);border:1px solid var(--success-border)}
.ehs-hv{background:var(--gold-bg);color:var(--gold);border:1px solid var(--gold-border);box-shadow:0 0 8px rgba(251,191,36,.3)}
.ehs-wait{background:var(--warning-bg);color:var(--warning);border:1px solid var(--warning-border)}
.ehs-warn{background:rgba(245,158,11,.15);color:#fcd34d;border:1px solid var(--warning-border)}
.ehs-no{background:var(--danger-bg);color:var(--danger);border:1px solid var(--danger-border)}
.entry-body{display:none;padding:0 var(--sp-5) var(--sp-5);border-top:1px solid var(--border);background:linear-gradient(180deg,var(--surface),var(--surface-2))}
.entry.open .entry-body{display:block;animation:fadeIn .3s ease}
@keyframes fadeIn{from{opacity:0;transform:translateY(-4px)}to{opacity:1;transform:translateY(0)}}
.section{margin-top:var(--sp-4)}
.section-h{font-size:var(--fs-xs);color:var(--brand-2);text-transform:uppercase;letter-spacing:.8px;font-weight:800;margin-bottom:var(--sp-3);display:flex;align-items:center;gap:6px}
.section-h.gold{color:var(--gold)}
.section-h.suc{color:var(--success)}
.motivo{padding:var(--sp-4) var(--sp-5);background:linear-gradient(135deg,rgba(59,130,246,.08),rgba(139,92,246,.06));border:1px solid rgba(59,130,246,.25);border-radius:var(--r-md);font-size:var(--fs-sm);line-height:1.7;color:var(--text-secondary)}
.motivo b{color:var(--text-primary);font-weight:700}
.motivo .hi{color:var(--success);font-weight:700}
.motivo .lo{color:var(--danger);font-weight:700}
.motivo .mid{color:var(--warning);font-weight:700}
.motivo .gold{color:var(--gold);font-weight:700}
.motivo p{margin-bottom:var(--sp-3)}
.motivo p:last-child{margin-bottom:0}
.entry-status-panel{margin-top:var(--sp-4);padding:var(--sp-4);background:var(--surface-3);border:2px solid var(--border-strong);border-radius:var(--r-md)}
.entry-status-panel.os-execute{border-color:var(--success);background:linear-gradient(135deg,rgba(16,185,129,.1),var(--surface-3))}
.entry-status-panel.os-hv{border-color:var(--gold);background:linear-gradient(135deg,rgba(251,191,36,.1),var(--surface-3));box-shadow:0 0 16px rgba(251,191,36,.15)}
.entry-status-panel.os-wait{border-color:var(--warning);background:linear-gradient(135deg,rgba(245,158,11,.1),var(--surface-3))}
.entry-status-panel.os-warn{border-color:#fcd34d}
.entry-status-panel.os-no{border-color:var(--danger);background:linear-gradient(135deg,rgba(239,68,68,.1),var(--surface-3))}
.entry-status-panel-h{font-size:var(--fs-xs);text-transform:uppercase;letter-spacing:.6px;font-weight:800;margin-bottom:var(--sp-3);color:var(--text-tertiary)}
.entry-status-decision{display:flex;align-items:center;gap:var(--sp-3);margin-bottom:var(--sp-3)}
.entry-status-decision .ic{font-size:var(--fs-2xl);flex-shrink:0}
.entry-status-decision .txt{flex:1}
.entry-status-decision .lbl{display:block;font-weight:800;font-size:var(--fs-md);text-transform:uppercase;letter-spacing:.4px;margin-bottom:2px}
.os-execute .entry-status-decision .lbl{color:var(--success)}
.os-hv .entry-status-decision .lbl{color:var(--gold)}
.os-wait .entry-status-decision .lbl{color:var(--warning)}
.os-warn .entry-status-decision .lbl{color:#fcd34d}
.os-no .entry-status-decision .lbl{color:var(--danger)}
.entry-status-decision .msg{font-size:var(--fs-sm);color:var(--text-secondary);line-height:1.5}
.entry-status-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:var(--sp-2);margin-top:var(--sp-3)}
.entry-status-grid .b{background:var(--bg-elevated);padding:var(--sp-2) var(--sp-3);border-radius:var(--r-sm);border:1px solid var(--border)}
.entry-status-grid .b .k{font-size:9px;color:var(--text-tertiary);text-transform:uppercase;letter-spacing:.4px;font-weight:700}
.entry-status-grid .b .v{font-family:'JetBrains Mono';font-weight:800;font-size:var(--fs-sm);color:var(--text-primary);margin-top:2px}
.entry-status-grid .b .v.pos{color:var(--success)}
.entry-status-grid .b .v.neg{color:var(--danger)}
.entry-status-empty{padding:var(--sp-3);text-align:center;color:var(--text-tertiary);font-size:var(--fs-sm);font-style:italic}
.req-grid{display:grid;grid-template-columns:1fr 1fr;gap:var(--sp-3)}
@media(max-width:640px){.req-grid{grid-template-columns:1fr}}
.req-box{padding:var(--sp-4);background:var(--surface-3);border:1px solid var(--border);border-radius:var(--r-md);border-left:4px solid}
.req-box.entrada{border-left-color:var(--success)}
.req-box.saida{border-left-color:var(--warning)}
.req-box h5{font-size:var(--fs-xs);text-transform:uppercase;letter-spacing:.6px;font-weight:800;margin-bottom:var(--sp-3);display:flex;align-items:center;gap:6px}
.req-box.entrada h5{color:var(--success)}
.req-box.saida h5{color:var(--warning)}
.req-box ul{list-style:none}
.req-box li{padding:var(--sp-2) 0;border-bottom:1px dashed var(--border);font-size:var(--fs-sm);color:var(--text-secondary);display:flex;justify-content:space-between;gap:var(--sp-3);align-items:center}
.req-box li:last-child{border:none}
.req-box li .k{color:var(--text-tertiary);font-size:var(--fs-xs);text-transform:uppercase;letter-spacing:.4px;font-weight:600}
.req-box li .v{font-family:'JetBrains Mono';font-weight:700;color:var(--text-primary);text-align:right}
.odds-box{display:grid;grid-template-columns:repeat(3,1fr);gap:var(--sp-2);margin-top:var(--sp-3)}
.ob{background:var(--surface-3);padding:var(--sp-3);border-radius:var(--r-md);text-align:center;border:1px solid var(--border)}
.ob .ol{font-size:var(--fs-2xs);color:var(--text-tertiary);text-transform:uppercase;letter-spacing:.5px;font-weight:700;margin-bottom:4px}
.ob .ov{font-family:'JetBrains Mono';font-size:var(--fs-lg);font-weight:800;color:var(--brand-2)}
.ob.ideal{border-color:var(--gold-border);background:var(--gold-bg)}
.ob.ideal .ov{color:var(--gold)}
.crit{list-style:none;margin-top:var(--sp-3);background:var(--surface-3);border:1px solid var(--border);border-radius:var(--r-md);overflow:hidden}
.crit li{padding:var(--sp-3) var(--sp-4);border-bottom:1px solid var(--border);display:flex;align-items:flex-start;gap:var(--sp-3);font-size:var(--fs-sm);color:var(--text-secondary)}
.crit li:last-child{border:none}
.crit li .ic{flex-shrink:0;width:24px;height:24px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:900;font-family:'JetBrains Mono';font-size:var(--fs-xs)}
.crit li.ok .ic{background:var(--success-bg);color:var(--success);border:1px solid var(--success-border)}
.crit li.no .ic{background:var(--danger-bg);color:var(--danger);border:1px solid var(--danger-border)}
.crit li.no{opacity:.65}
.crit li .txt{flex:1;line-height:1.5}
.crit li .w{font-size:var(--fs-2xs);background:var(--surface-2);color:var(--text-tertiary);padding:2px 6px;border-radius:var(--r-sm);font-family:'JetBrains Mono';font-weight:700;flex-shrink:0;border:1px solid var(--border)}
#toast{position:fixed;bottom:var(--sp-5);right:var(--sp-5);z-index:300;padding:var(--sp-3) var(--sp-5);background:var(--success);color:#fff;border-radius:var(--r-md);font-weight:600;font-size:var(--fs-sm);box-shadow:var(--sh-xl);display:none;animation:slideIn .35s cubic-bezier(.4,0,.2,1);max-width:380px}
#toast.err{background:var(--danger)}
@keyframes slideIn{from{transform:translateX(120%);opacity:0}to{transform:translateX(0);opacity:1}}
.modal{display:none;position:fixed;inset:0;background:rgba(0,0,0,.85);z-index:200;align-items:center;justify-content:center;padding:var(--sp-5);backdrop-filter:blur(8px)}
.modal.on{display:flex;animation:fadeIn .25s ease}
.modal-c{background:var(--surface);border-radius:var(--r-xl);padding:var(--sp-6);max-width:640px;width:100%;max-height:85vh;overflow-y:auto;border:1px solid var(--border);box-shadow:var(--sh-xl)}
.modal-c h3{margin-bottom:var(--sp-3);color:var(--brand-2);font-size:var(--fs-lg)}
.modal-c .desc{font-size:var(--fs-sm);color:var(--text-secondary);line-height:1.5;margin-bottom:var(--sp-4)}
.modal-c .desc b{color:var(--text-primary)}
.modal-c .desc code{background:var(--surface-3);padding:2px 6px;border-radius:var(--r-sm);font-family:'JetBrains Mono';font-size:var(--fs-xs);color:var(--gold);border:1px solid var(--border)}
.modal-c textarea{width:100%;min-height:160px;background:var(--bg-elevated);border:1px solid var(--border-strong);color:var(--text-primary);padding:var(--sp-3);border-radius:var(--r-md);font-family:'JetBrains Mono';font-size:var(--fs-sm);font-weight:600;line-height:1.5;resize:vertical;margin-bottom:var(--sp-3)}
.modal-c textarea:focus{outline:none;border-color:var(--brand)}
.modal-c .preview{background:var(--bg-elevated);border:1px solid var(--border);border-radius:var(--r-md);padding:var(--sp-3);margin-bottom:var(--sp-3);font-size:var(--fs-xs);color:var(--text-tertiary);font-family:'JetBrains Mono';max-height:200px;overflow-y:auto}
.modal-c .preview .row{display:flex;justify-content:space-between;gap:var(--sp-2);padding:4px 0;border-bottom:1px dashed var(--border)}
.modal-c .preview .row:last-child{border:none}
.modal-c .preview .row.matched{color:var(--success)}
.modal-c .preview .row.matched b{color:var(--lime)}
.modal-c .preview .row.skipped{opacity:.5}
.modal-c .preview b{color:var(--text-primary);font-weight:800}
.modal-c .preview-empty{color:var(--text-tertiary);font-style:italic;text-align:center;padding:var(--sp-2)}
.modal-c .mode-tabs{display:inline-flex;gap:0;background:var(--bg-elevated);border-radius:var(--r-md);padding:3px;margin-bottom:var(--sp-3);border:1px solid var(--border)}
.modal-c .mode-tab{padding:6px 12px;background:none;border:none;color:var(--text-tertiary);font-size:var(--fs-xs);font-weight:700;cursor:pointer;border-radius:var(--r-sm);transition:var(--t-fast);font-family:inherit;text-transform:uppercase;letter-spacing:.4px}
.modal-c .mode-tab.on{background:var(--brand);color:#fff}
.chk-lbl{display:flex;align-items:center;gap:var(--sp-2);font-size:var(--fs-sm);cursor:pointer;padding:var(--sp-2);border-radius:var(--r-sm)}
.chk-lbl:hover{background:var(--surface-2)}
/* Settings - market selector */
.sett-section{margin-top:var(--sp-5);padding-top:var(--sp-4);border-top:1px solid var(--border)}
.sett-section h4{font-size:var(--fs-sm);color:var(--gold);margin-bottom:var(--sp-2);display:flex;align-items:center;gap:6px;font-weight:800}
.sett-section .sett-desc{font-size:var(--fs-xs);color:var(--text-tertiary);margin-bottom:var(--sp-3);line-height:1.5}
.mkt-group{margin-bottom:var(--sp-3)}
.mkt-group-h{font-size:10px;color:var(--brand-2);text-transform:uppercase;letter-spacing:.6px;font-weight:800;margin-bottom:var(--sp-2);padding-left:2px;display:flex;align-items:center;justify-content:space-between}
.mkt-group-h .actions{display:flex;gap:6px}
.mkt-group-h button{background:none;border:1px solid var(--border-strong);color:var(--text-tertiary);font-size:9px;font-weight:700;padding:2px 6px;border-radius:var(--r-sm);cursor:pointer;text-transform:uppercase;letter-spacing:.3px}
.mkt-group-h button:hover{color:var(--text-primary);border-color:var(--brand)}
.mkt-list{display:grid;grid-template-columns:repeat(auto-fill,minmax(140px,1fr));gap:6px}
.mkt-chk{display:flex;align-items:center;gap:6px;padding:6px 10px;border-radius:var(--r-sm);background:var(--bg-elevated);border:1px solid var(--border);cursor:pointer;font-size:var(--fs-xs);font-weight:600;transition:var(--t-fast)}
.mkt-chk:hover{border-color:var(--brand-2)}
.mkt-chk input{margin:0;cursor:pointer}
.mkt-chk.on{background:var(--success-bg);border-color:var(--success-border);color:var(--success)}
.mkt-chk .tagk{font-family:'JetBrains Mono';font-size:9px;padding:1px 4px;border-radius:3px;background:var(--surface-3);color:var(--text-tertiary);margin-left:auto}
.mkt-chk.on .tagk{background:var(--success-bg);color:var(--success)}
.mkt-quickbtns{display:flex;gap:6px;margin-bottom:var(--sp-3);flex-wrap:wrap}
.mkt-quickbtns button{font-size:var(--fs-xs);padding:6px 10px;background:var(--surface-3);border:1px solid var(--border-strong);color:var(--text-secondary);border-radius:var(--r-sm);cursor:pointer;font-weight:700;font-family:inherit}
.mkt-quickbtns button:hover{background:var(--brand);color:#fff;border-color:var(--brand)}
.empty{text-align:center;padding:var(--sp-12) var(--sp-5);color:var(--text-tertiary)}
.empty-ic{font-size:64px;margin-bottom:var(--sp-4);opacity:.5}
.empty h3{font-size:var(--fs-lg);color:var(--text-secondary);margin-bottom:var(--sp-2);font-weight:700}
.empty p{font-size:var(--fs-sm);max-width:420px;margin:0 auto var(--sp-4)}
::-webkit-scrollbar{width:10px;height:10px}
::-webkit-scrollbar-track{background:var(--bg)}
::-webkit-scrollbar-thumb{background:var(--surface-3);border-radius:5px}
@media(max-width:640px){.app{padding:var(--sp-3)}.hdr{padding:var(--sp-4)}.hdr h1{font-size:var(--fs-lg)}.m-head{padding:var(--sp-4)}.pois-banner{padding:var(--sp-3) var(--sp-4)}.entries,.match-odds{padding:var(--sp-4)}.entry-head{padding:var(--sp-3) var(--sp-4);padding-left:38px}.entry-body{padding:0 var(--sp-4) var(--sp-4)}.entry-num{width:40px;height:40px;font-size:var(--fs-sm)}.entry-score .n{font-size:var(--fs-xl)}.m-teams{font-size:var(--fs-lg)}.chip{padding:5px 10px;font-size:10px}}

/* === v7.3.0 — Flat grid + sidebar === */
.view-tabs{display:inline-flex;gap:0;background:var(--bg-elevated);border-radius:var(--r-md);padding:3px;margin-bottom:var(--sp-4);border:1px solid var(--border)}
.view-tab{padding:8px 16px;background:none;border:none;color:var(--text-tertiary);font-size:var(--fs-sm);font-weight:700;cursor:pointer;border-radius:var(--r-sm);transition:var(--t-fast);font-family:inherit;letter-spacing:.3px;display:inline-flex;align-items:center;gap:6px}
.view-tab.on{background:var(--brand);color:#fff;box-shadow:0 2px 8px rgba(59,130,246,.3)}
.view-tab:not(.on):hover{color:var(--text-primary);background:var(--surface-2)}
.results-wrap{display:grid;grid-template-columns:260px 1fr;gap:var(--sp-4);align-items:start}
@media(max-width:960px){.results-wrap{grid-template-columns:1fr}}
.side-menu{background:linear-gradient(135deg,var(--surface),var(--bg-elevated));border:1px solid var(--border);border-radius:var(--r-xl);padding:var(--sp-4);position:sticky;top:var(--sp-4);max-height:calc(100vh - var(--sp-8));overflow-y:auto;box-shadow:var(--sh-md)}
@media(max-width:960px){.side-menu{position:static;max-height:none}}
.side-h{font-size:var(--fs-sm);font-weight:800;color:var(--gold);margin-bottom:var(--sp-3);text-transform:uppercase;letter-spacing:.5px;display:flex;align-items:center;gap:6px}
.side-quick{display:flex;gap:6px;margin-bottom:var(--sp-3)}
.side-quick button{flex:1;padding:6px;background:var(--surface-3);border:1px solid var(--border-strong);color:var(--text-secondary);border-radius:var(--r-sm);font-size:10px;font-weight:700;cursor:pointer;font-family:inherit;text-transform:uppercase;letter-spacing:.3px}
.side-quick button:hover{background:var(--brand);color:#fff;border-color:var(--brand)}
.side-section-h{font-size:9px;color:var(--text-tertiary);text-transform:uppercase;letter-spacing:.6px;font-weight:800;margin:var(--sp-4) 0 var(--sp-2);padding-left:2px}
.side-methods{display:flex;flex-direction:column;gap:4px}
.side-method-chk{display:grid;grid-template-columns:auto 32px 1fr auto;align-items:center;gap:6px;padding:6px 8px;background:var(--bg-elevated);border:1px solid var(--border);border-radius:var(--r-sm);cursor:pointer;transition:var(--t-fast)}
.side-method-chk:hover{border-color:var(--brand-2)}
.side-method-chk.on{background:rgba(16,185,129,.08);border-color:var(--success-border)}
.side-method-chk.empty{opacity:.45}
.side-method-chk input{margin:0;cursor:pointer;accent-color:var(--success)}
.side-method-num{font-family:'JetBrains Mono';font-weight:800;font-size:9px;background:var(--surface-3);padding:2px 5px;border-radius:3px;color:var(--brand-2);text-align:center}
.side-method-chk.on .side-method-num{background:var(--success-bg);color:var(--success)}
.side-method-title{font-size:11px;color:var(--text-secondary);line-height:1.25;font-weight:600}
.side-method-count{font-family:'JetBrains Mono';font-weight:800;font-size:10px;color:var(--text-tertiary);min-width:18px;text-align:right;padding:2px 5px;background:var(--surface-3);border-radius:3px}
.side-method-chk.on .side-method-count{color:var(--success);background:var(--success-bg)}
.side-extra{display:flex;flex-direction:column;gap:6px;margin-top:var(--sp-3)}
.side-extra .chk-lbl{padding:6px 8px;font-size:11px;font-weight:600;background:var(--bg-elevated);border:1px solid var(--border);border-radius:var(--r-sm)}
.flat-grid{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:var(--sp-3)}
@media(max-width:1280px){.flat-grid{grid-template-columns:repeat(3,minmax(0,1fr))}}
@media(max-width:960px){.flat-grid{grid-template-columns:repeat(2,minmax(0,1fr))}}
@media(max-width:640px){.flat-grid{grid-template-columns:1fr}}
.ecard{background:linear-gradient(135deg,var(--surface-2),var(--surface));border:1px solid var(--border);border-radius:var(--r-lg);overflow:hidden;transition:var(--t-base);position:relative;display:flex;flex-direction:column;min-width:0}
.ecard:hover{border-color:var(--border-strong);transform:translateY(-2px);box-shadow:var(--sh-md)}
.ecard.hv{border:2px solid transparent;background:linear-gradient(var(--surface-2),var(--surface-2)) padding-box,linear-gradient(135deg,var(--gold),#f59e0b,var(--gold)) border-box;background-size:200% 100%;animation:shineGold 3s linear infinite}
.ecard.s-no{opacity:.55}
.ecard.s-no:hover{opacity:.85}
.ecard.s-wait{border-color:var(--warning)}
.ecard.s-execute,.ecard.s-hv{box-shadow:0 0 14px rgba(16,185,129,.18)}
.ecard.open{grid-column:1/-1}
.ecard-rank{position:absolute;top:6px;right:6px;width:22px;height:22px;border-radius:50%;background:var(--brand);color:#fff;display:flex;align-items:center;justify-content:center;font-family:'JetBrains Mono';font-weight:900;font-size:10px;z-index:2;box-shadow:var(--sh-sm)}
.ecard-rank.r1{background:linear-gradient(135deg,var(--gold),#f59e0b);color:#1a1000;box-shadow:0 0 10px rgba(251,191,36,.6)}
.ecard-rank.r2{background:linear-gradient(135deg,#c0c0c0,#8b8b8b);color:#1a1a1a}
.ecard-rank.r3{background:linear-gradient(135deg,#cd7f32,#8b4513);color:#fff}
.ecard-match{padding:8px 10px;background:linear-gradient(135deg,rgba(59,130,246,.12),rgba(139,92,246,.06));border-bottom:1px solid var(--border)}
.ecard-match-top{display:flex;justify-content:space-between;align-items:center;gap:6px;margin-bottom:3px}
.ecard-match-teams{font-size:12px;font-weight:800;letter-spacing:-.1px;flex:1;min-width:0;line-height:1.2;color:var(--text-primary);overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.ecard-match-teams .vs{color:var(--text-tertiary);font-weight:400;margin:0 4px;font-size:10px}
.ecard-match-bot{display:flex;justify-content:space-between;align-items:center;gap:6px;font-size:9px;color:var(--text-tertiary);font-family:'JetBrains Mono'}
.ecard-match-dt,.ecard-match-comp{white-space:nowrap;overflow:hidden;text-overflow:ellipsis;min-width:0}
.ecard-live{font-size:8px;font-weight:800;color:#fca5a5;background:rgba(239,68,68,.18);padding:2px 6px;border-radius:4px;letter-spacing:.3px;animation:livePulse 2s ease-in-out infinite;flex-shrink:0}
.ecard-soon{font-size:8px;font-weight:800;color:#fcd34d;background:rgba(245,158,11,.18);padding:2px 6px;border-radius:4px;letter-spacing:.3px;flex-shrink:0}
.ecard-method{padding:10px;display:flex;gap:8px;align-items:flex-start}
.ecard-method-num{width:32px;height:32px;background:linear-gradient(135deg,var(--brand),var(--accent));color:#fff;border-radius:var(--r-sm);display:flex;align-items:center;justify-content:center;font-family:'JetBrains Mono';font-weight:800;font-size:11px;flex-shrink:0}
.ecard.hv .ecard-method-num{background:linear-gradient(135deg,var(--gold),#f59e0b);color:#1a1000}
.ecard-method-text{flex:1;min-width:0}
.ecard-method-title{font-weight:700;font-size:12px;line-height:1.25;letter-spacing:-.2px;color:var(--text-primary)}
.ecard-method-title .star{color:var(--gold);font-size:10px}
.ecard-method-mkt{display:flex;align-items:center;gap:5px;margin-top:3px;font-size:9px;color:var(--text-tertiary);flex-wrap:wrap}
.ecard-method-mkt .tag{padding:1px 5px;border-radius:3px;background:rgba(6,182,212,.1);color:var(--brand-2);font-weight:600;border:1px solid rgba(6,182,212,.25);text-transform:uppercase;font-family:'JetBrains Mono';font-size:8px;letter-spacing:.3px}
.ecard-method-mkt .tag.back{background:var(--success-bg);color:var(--success);border-color:var(--success-border)}
.ecard-method-mkt .tag.lay{background:rgba(236,72,153,.1);color:#f9a8d4;border-color:rgba(236,72,153,.3)}
.ecard-stats{display:grid;grid-template-columns:1fr 1fr;gap:8px;padding:0 10px 10px;align-items:center}
.ecard-stat{display:flex;flex-direction:column;gap:1px}
.ecard-stat .lbl{font-size:8px;color:var(--text-tertiary);text-transform:uppercase;letter-spacing:.4px;font-weight:700}
.ecard-stat .v{font-family:'JetBrains Mono';font-weight:800;font-size:14px;line-height:1.1}
.ecard-stat .score-v{color:var(--success)}
.ecard-stat .prob{padding:2px 6px;border-radius:3px;display:inline-block;width:fit-content;font-size:11px;font-family:'JetBrains Mono';font-weight:800}
.ecard-stat .prob.high{background:rgba(132,204,22,.12);color:var(--lime);border:1px solid rgba(132,204,22,.3)}
.ecard-stat .prob.mid{background:var(--warning-bg);color:var(--warning);border:1px solid var(--warning-border)}
.ecard-stat .prob.low{background:var(--danger-bg);color:var(--danger);border:1px solid var(--danger-border)}
.ecard-status-row{padding:0 10px 8px}
.ecard-status{padding:4px 8px;border-radius:999px;font-size:9px;font-weight:800;font-family:'JetBrains Mono';white-space:nowrap;letter-spacing:.3px;width:100%;text-align:center;display:inline-block;box-sizing:border-box}
.ecard-status.ehs-execute{background:var(--success-bg);color:var(--success);border:1px solid var(--success-border)}
.ecard-status.ehs-hv{background:var(--gold-bg);color:var(--gold);border:1px solid var(--gold-border);box-shadow:0 0 6px rgba(251,191,36,.3)}
.ecard-status.ehs-wait{background:var(--warning-bg);color:var(--warning);border:1px solid var(--warning-border)}
.ecard-status.ehs-warn{background:rgba(245,158,11,.15);color:#fcd34d;border:1px solid var(--warning-border)}
.ecard-status.ehs-no{background:var(--danger-bg);color:var(--danger);border:1px solid var(--danger-border)}
.ecard-status.empty{background:var(--surface-3);color:var(--text-tertiary);border:1px dashed var(--border)}
.ecard-expand-btn{margin:0;padding:8px;background:var(--surface-3);border:none;border-top:1px solid var(--border);color:var(--brand-2);font-size:10px;font-weight:800;cursor:pointer;letter-spacing:.4px;text-transform:uppercase;transition:var(--t-fast);font-family:inherit}
.ecard-expand-btn:hover{background:var(--brand);color:#fff}
.ecard.open .ecard-expand-btn{background:var(--brand);color:#fff}
.ecard-body{padding:12px;border-top:1px solid var(--border);background:linear-gradient(180deg,var(--surface),var(--surface-2));animation:fadeIn .3s ease}
.ecard-odd-input{display:flex;gap:8px;align-items:center;background:var(--surface-3);padding:10px;border-radius:var(--r-md);border:1px solid var(--border);flex-wrap:wrap}
.ecard-odd-input label{font-size:11px;color:var(--text-tertiary);text-transform:uppercase;letter-spacing:.4px;font-weight:700;white-space:nowrap}
.ecard-odd-input input{flex:1;background:var(--bg-elevated);border:1px solid var(--border-strong);color:var(--text-primary);font-family:'JetBrains Mono';font-size:16px;font-weight:800;padding:6px 8px;border-radius:var(--r-sm);text-align:center;min-width:80px;max-width:120px}
.ecard-odd-input input:focus{outline:none;border-color:var(--brand)}

</style>
</head>
<body>
<div class="app">
<header class="hdr"><div class="hdr-brand"><div class="hdr-logo" id="brandLogo">AO</div><div><h1>Analisador Operacional</h1><div class="sub">v7.3.1 • Lay 1×0/0×1 faixa 9-15 • Back HV sem teto</div></div></div><div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap"><span class="hdr-pill" id="clk">🕐 --:--:--</span><button class="btn alt sm" onclick="openSettings()">⚙</button></div></header>
<div class="upload" id="upl"><h3>📤 Envie os PDFs estatísticos das partidas</h3><p>Arraste e solte ou clique</p><input type="file" id="fIn" multiple accept="application/pdf" style="display:none"><input type="file" id="logoIn" accept="image/png,image/jpeg,image/svg+xml" style="display:none"><div class="upload-actions"><button class="btn" onclick="document.getElementById('fIn').click()">📁 Selecionar PDFs</button><button class="btn suc" id="procBtn" onclick="processAll()" disabled>⚙ Processar Análise</button><button class="btn alt" onclick="clearAll()">🗑 Limpar</button></div><ul class="fl" id="fl"></ul></div>
<div id="results" style="display:none">
<div class="tools"><div class="tools-info"><span class="badge suc">✓ <span id="totConf">0</span> ENTRADAS</span><span class="badge gld">⭐ <span id="totHv">0</span> HV</span><span class="badge warn">📋 <span id="totMatches">0</span> Partidas</span><span class="badge suc" id="badgeApproved" style="display:none">▶ <span id="totApproved">0</span> APROVADAS</span><span class="badge warn" id="badgeWait" style="display:none">⏳ <span id="totWait">0</span> AGUARDAR</span><span class="badge dng" id="badgeReject" style="display:none">⛔ <span id="totReject">0</span> REJEITADAS</span></div><div style="display:flex;gap:8px;flex-wrap:wrap"><button class="btn dng sm" onclick="exportPDF()">📄 PDF</button><button class="btn inf sm" onclick="exportCSV()">📊 CSV</button></div></div>
<div class="view-tabs"><button class="view-tab on" data-view="grid" onclick="setView('grid')">📋 Por Entrada (Grid)</button><button class="view-tab" data-view="match" onclick="setView('match')">🎯 Por Partida</button></div>
<div id="viewGrid" class="results-wrap">
<aside id="sideMenu" class="side-menu"></aside>
<div id="flatGrid" class="flat-grid"></div>
</div>
<div id="viewMatch" style="display:none">
<div style="margin-bottom:12px"><button class="btn alt sm" onclick="toggleAll()">↕ Expandir/Colapsar</button></div>
<div id="ml"></div>
</div>
<div id="emptyConf" class="empty" style="display:none"><div class="empty-ic">🔎</div><h3>Nenhuma entrada confirmada</h3><p>Ative "Incluir POSSÍVEIS" no menu lateral ou em ⚙.</p></div>
</div>
<div id="emptyState" class="empty"><div class="empty-ic">📊</div><h3>Aguardando PDFs estatísticos</h3><p>Envie um ou mais PDFs com dados pré-jogo.</p></div>
</div>
<div class="modal" id="settingsModal"><div class="modal-c"><h3>⚙ Configurações</h3><button class="btn alt" onclick="document.getElementById('logoIn').click()" style="width:100%;margin-bottom:12px">🖼 Carregar Logo</button><label class="chk-lbl"><input type="checkbox" id="incPos"> Incluir POSSÍVEIS</label><label class="chk-lbl"><input type="checkbox" id="autoOpen" checked> Auto-expandir 1ª entrada</label>
<div class="sett-section">
<h4>🎯 Mercados sempre visíveis no painel da partida</h4>
<div class="sett-desc">Escolha quais mercados aparecerão <b>em todas as partidas</b>, independente dos métodos confirmados. Os mercados dos métodos confirmados continuam aparecendo automaticamente.</div>
<div class="mkt-quickbtns"><button onclick="mktPreset('default')">Padrão (1X2+BTTS+O2.5)</button><button onclick="mktPreset('basic')">Básico (1X2)</button><button onclick="mktPreset('goals')">Foco em Gols</button><button onclick="mktPreset('all')">Todos</button><button onclick="mktPreset('none')">Nenhum</button></div>
<div id="mktSelector"></div>
</div>
<div style="margin-top:16px;display:flex;gap:8px"><button class="btn suc" onclick="saveSettings()">Salvar</button><button class="btn alt" onclick="closeSettings()">Fechar</button></div></div></div>
<div class="modal" id="pasteModal"><div class="modal-c"><h3>📋 Colar odds em massa</h3><div class="desc">Cole as odds copiadas. O sistema detecta e distribui.</div><div class="mode-tabs"><button class="mode-tab on" data-mode="smart" onclick="setPasteMode('smart')">🤖 Inteligente</button><button class="mode-tab" data-mode="sequence" onclick="setPasteMode('sequence')">📝 Sequência</button></div><div class="desc" id="pasteDesc">Detecta nomes + odds: <code>Casa 1.85 X 3.40 Fora 4.20</code></div><textarea id="pasteInput" placeholder="Cole aqui..." oninput="updatePastePreview()"></textarea><div class="preview" id="pastePreview"><div class="preview-empty">Cole as odds acima</div></div><div style="display:flex;gap:8px;flex-wrap:wrap;align-items:center"><button class="btn suc" onclick="applyPaste()">✓ Aplicar</button><button class="btn alt" onclick="closePaste()">Cancelar</button><div style="margin-left:auto;font-size:11px;color:var(--text-tertiary);font-family:'JetBrains Mono'"><span id="pasteCount">0</span></div></div></div></div>
<div id="toast"></div>
<script>
var pdfjsLib=window['pdfjs-dist/build/pdf']||window.pdfjsLib;
if(pdfjsLib){pdfjsLib.GlobalWorkerOptions.workerSrc='https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js'}
var FILES=[],AR=[];
var LOGO_B64=localStorage.getItem('company_logo')||null;
var INC_POS=localStorage.getItem('inc_pos')==='true';
var AUTO_OPEN=localStorage.getItem('auto_open')!=='false';
var DT_OVERRIDES=JSON.parse(localStorage.getItem('dt_overrides')||'{}');
var MATCH_ODDS=JSON.parse(localStorage.getItem('match_odds')||'{}');
var PASTE_CTX={idx:null,mode:'smart',parsed:[]};
/* === v7.3.0 — flat grid state === */
var METHOD_FILTER=(function(){var s=localStorage.getItem('method_filter');if(s){try{var a=JSON.parse(s);if(Array.isArray(a))return a}catch(e){}}return [1,2,3,4,5,6,9,10,11,12,13]})();
var ONLY_HV=localStorage.getItem('only_hv')==='true';
var ONLY_APPROVED=localStorage.getItem('only_approved')==='true';
var CURRENT_VIEW=localStorage.getItem('current_view')||'grid';

/* NEW: Always-show markets, configurable */
var DEFAULT_ALWAYS_SHOW = ['home','draw','away','bttsYes','bttsNo','over25','under25'];
var ALWAYS_SHOW_KEYS = (function(){
  var saved = localStorage.getItem('always_show_keys');
  if(saved){try{var arr=JSON.parse(saved);if(Array.isArray(arr))return arr}catch(e){}}
  return DEFAULT_ALWAYS_SHOW.slice();
})();

function $(s){return document.querySelector(s)}
function $$(s){return document.querySelectorAll(s)}
function fmt(n,d){d=d===undefined?2:d;if(n==null||isNaN(n))return '-';return Number(n).toFixed(d)}
function escHtml(s){if(s==null)return '';return String(s).replace(/[&<>"']/g,function(c){return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]})}
function toast(msg,err){var t=$('#toast');t.textContent=msg;t.className=err?'err':'';t.style.display='block';setTimeout(function(){t.style.display='none'},3000)}
function factorial(n){if(n<=1)return 1;var r=1;for(var i=2;i<=n;i++)r*=i;return r}
function poissonPMF(l,k){if(l<=0)return k===0?1:0;return Math.exp(-l)*Math.pow(l,k)/factorial(k)}
function clock(){var d=new Date();var el=$('#clk');if(el)el.innerHTML='🕐 '+d.toLocaleString('pt-BR',{day:'2-digit',month:'2-digit',hour:'2-digit',minute:'2-digit',second:'2-digit'})}
setInterval(clock,1000);clock();
function applyLogo(){var el=$('#brandLogo');if(LOGO_B64){el.innerHTML='<img src="'+LOGO_B64+'" alt="logo">'}else{el.innerHTML='AO'}}
function matchKey(d){return (d.home+'__'+d.away+'__'+(d.fileName||'')).replace(/\s+/g,'_').toLowerCase()}
function normalizeDate(s){if(!s)return '';s=s.trim();var m=s.match(/^(\d{1,2})[\/-](\d{1,2})[\/-](\d{2,4})$/);if(m){var dd=m[1].padStart(2,'0'),mm=m[2].padStart(2,'0'),yy=m[3];if(yy.length===2)yy='20'+yy;return dd+'/'+mm+'/'+yy}var m2=s.match(/^(\d{4})-(\d{2})-(\d{2})$/);if(m2)return m2[3]+'/'+m2[2]+'/'+m2[1];return s}
function normalizeTime(s){if(!s)return '';s=s.trim();var m=s.match(/^(\d{1,2}):(\d{2})$/);if(m){return m[1].padStart(2,'0')+':'+m[2]}return s}
function parseDateTime(date,time){if(!date)return null;var m=date.match(/^(\d{2})\/(\d{2})\/(\d{4})$/);if(!m)return null;var h=12,min=0;if(time){var mt=time.match(/^(\d{1,2}):(\d{2})$/);if(mt){h=parseInt(mt[1]);min=parseInt(mt[2])}}return new Date(parseInt(m[3]),parseInt(m[2])-1,parseInt(m[1]),h,min)}
function timeStatus(date,time){var dt=parseDateTime(date,time);if(!dt)return '';var now=new Date();var diffMin=(dt-now)/60000;if(diffMin>=-120 && diffMin<=15)return 'live';if(diffMin>0 && diffMin<=60)return 'soon';if(diffMin<-120)return 'past';return ''}

var MARKET_LIBRARY={
  home:{group:'1X2',label:'1 — Casa vence',short:'Vit. Casa',type:'back',aliases:['casa','1','home','mandante','vitoria casa','vitória casa']},
  draw:{group:'1X2',label:'X — Empate',short:'Empate',type:'back',aliases:['empate','x','draw','tie']},
  away:{group:'1X2',label:'2 — Fora vence',short:'Vit. Fora',type:'back',aliases:['fora','2','away','visitante','vitoria fora','vitória fora']},
  over15:{group:'Gols',label:'Over 1.5',short:'Mais 1.5',type:'back',aliases:['over 1.5','+1.5','mais 1.5','mais de 1.5','o1.5','over1.5']},
  under15:{group:'Gols',label:'Under 1.5',short:'Menos 1.5',type:'back',aliases:['under 1.5','-1.5','menos 1.5','u1.5','under1.5']},
  over25:{group:'Gols',label:'Over 2.5',short:'Mais 2.5',type:'back',aliases:['over 2.5','+2.5','mais 2.5','mais de 2.5','o2.5','over2.5']},
  under25:{group:'Gols',label:'Under 2.5',short:'Menos 2.5',type:'back',aliases:['under 2.5','-2.5','menos 2.5','u2.5','under2.5']},
  over35:{group:'Gols',label:'Over 3.5',short:'Mais 3.5',type:'back',aliases:['over 3.5','+3.5','mais 3.5','mais de 3.5','o3.5','over3.5']},
  under35:{group:'Gols',label:'Under 3.5',short:'Menos 3.5',type:'back',aliases:['under 3.5','-3.5','menos 3.5','u3.5','under3.5']},
  bttsYes:{group:'BTTS',label:'Ambas marcam SIM',short:'BTTS SIM',type:'back',aliases:['btts sim','btts yes','ambas sim','ambas marcam sim','ambos marcam sim','gg','btts','ambas marcam']},
  bttsNo:{group:'BTTS',label:'Ambas marcam NÃO',short:'BTTS NÃO',type:'back',aliases:['btts nao','btts não','btts no','ambas nao','ambas não','ng']},
  layCS10:{group:'Correct Score (Lay)',label:'Lay 1×0',short:'Lay 1-0',type:'lay',aliases:['lay 1-0','lay 1x0','lay 1 0','lay10']},
  layCS01:{group:'Correct Score (Lay)',label:'Lay 0×1',short:'Lay 0-1',type:'lay',aliases:['lay 0-1','lay 0x1','lay 0 1','lay01']},
  layCS20:{group:'Correct Score (Lay)',label:'Lay 2×0',short:'Lay 2-0',type:'lay',aliases:['lay 2-0','lay 2x0','lay 2 0','lay20']},
  layCS02:{group:'Correct Score (Lay)',label:'Lay 0×2',short:'Lay 0-2',type:'lay',aliases:['lay 0-2','lay 0x2','lay 0 2','lay02']},
  layDraw:{group:'Lay 1X2',label:'Lay Empate',short:'Lay X',type:'lay',aliases:['lay empate','lay x','lay draw']},
  layHome:{group:'Lay 1X2',label:'Lay Casa (azarão)',short:'Lay Casa',type:'lay',aliases:['lay casa','lay home','lay 1','lay mandante']},
  layAway:{group:'Lay 1X2',label:'Lay Fora (azarão)',short:'Lay Fora',type:'lay',aliases:['lay fora','lay away','lay 2','lay visitante']},
  ah05Home:{group:'Asian Handicap',label:'AH -0.5 Casa',short:'AH -0.5 C',type:'back',aliases:['ah -0.5 casa','ah-0.5 casa','ah -0.5 home','handicap -0.5 casa']},
  ah05Away:{group:'Asian Handicap',label:'AH -0.5 Fora',short:'AH -0.5 F',type:'back',aliases:['ah -0.5 fora','ah-0.5 fora','ah -0.5 away','handicap -0.5 fora']},
  over05_2T:{group:'Live (2º Tempo)',label:'Over 0.5 gol 70-90min',short:'+0.5 70-90',type:'back',aliases:['over 0.5 2t','over 0.5 70','over 0.5 70-90','gol 70 90','+0.5 70-90']}
};
var MARKET_GROUPS=['1X2','Gols','BTTS','Lay 1X2','Correct Score (Lay)','Asian Handicap','Live (2º Tempo)'];
var METHOD_MARKETS={1:['home','away'],2:['over25','bttsYes'],3:['over35'],4:['over05_2T'],5:['layCS10'],6:['layCS01'],9:['layHome','layAway'],10:['home','away'],11:['over25','bttsYes'],12:['layDraw'],13:['ah05Home','ah05Away']};
var METHOD_LEGENDS={1:{icon:'🏆',label:'M1',title:'Back Favorito (Vitória 1X2)',ideal:'Favorito com PPG ≥1.2 acima, em casa'},2:{icon:'🔥',label:'M2',title:'Back 2×2 (BTTS + Over 2.5)',ideal:'BTTS >55%, Over 2.5 >55%, média ≥2.8 gols • odd quanto mais alta, melhor ⭐'},3:{icon:'⚽',label:'M3',title:'Back Goleada (Over 3.5)',ideal:'PPG diff ≥2, favorito gols/jogo ≥2.2 • odd quanto mais alta, melhor ⭐'},4:{icon:'⏱',label:'M4',title:'Over 70min (Live)',ideal:'Média 2T ≥1.3 gols, jogo equilibrado'},5:{icon:'🚫',label:'M5',title:'Lay 1×0',ideal:'P(1-0) <12%, visitante marca >60% • faixa odd 9-15'},6:{icon:'🚫',label:'M6',title:'Lay 0×1',ideal:'P(0-1) <12%, mandante marca >65% • faixa odd 9-15'},9:{icon:'🦓',label:'M9',title:'Lay Zebra',ideal:'Diff força ≥1.5, azarão win% <30%'},10:{icon:'⚡',label:'M10',title:'Momentum & Eficiência (Vitória)',ideal:'Momentum ≥60%, estilo DOMINANTE/OFENSIVO'},11:{icon:'🎯',label:'M10b',title:'Momentum & Eficiência (Gols)',ideal:'Over 2.5 e BTTS ≥55%, momentum médio ≥50%'},12:{icon:'❌',label:'M12',title:'Lay Empate',ideal:'P(empate)<25%, média gols ≥2.8'},13:{icon:'📐',label:'M13',title:'Asian Handicap -0.5',ideal:'PPG diff ≥1.5, favorito casa, momentum alto'}};

document.getElementById('fIn').addEventListener('change',function(e){addFiles(e.target.files)});
var upl=document.getElementById('upl');
['dragenter','dragover'].forEach(function(ev){upl.addEventListener(ev,function(e){e.preventDefault();upl.classList.add('dragover')})});
['dragleave','drop'].forEach(function(ev){upl.addEventListener(ev,function(e){e.preventDefault();upl.classList.remove('dragover')})});
upl.addEventListener('drop',function(e){addFiles(e.dataTransfer.files)});
function addFiles(fs){for(var i=0;i<fs.length;i++){if(fs[i].type==='application/pdf')FILES.push(fs[i])}renderFL()}
function renderFL(){var ul=$('#fl');ul.innerHTML='';FILES.forEach(function(f,i){var li=document.createElement('li');li.innerHTML='<span>📄 '+escHtml(f.name)+' <small style="color:var(--text-tertiary)">('+(f.size/1024).toFixed(0)+' KB)</small></span><span class="rm" onclick="rmF('+i+')">✕</span>';ul.appendChild(li)});$('#procBtn').disabled=FILES.length===0}
function rmF(i){FILES.splice(i,1);renderFL()}
function clearAll(){FILES=[];AR=[];$('#fl').innerHTML='';$('#results').style.display='none';$('#emptyState').style.display='';$('#procBtn').disabled=true}
document.getElementById('logoIn').addEventListener('change',function(e){var f=e.target.files[0];if(!f)return;if(f.size>500*1024){toast('Logo deve ter no máximo 500KB',true);return}var r=new FileReader();r.onload=function(){LOGO_B64=r.result;localStorage.setItem('company_logo',LOGO_B64);applyLogo();toast('🖼 Logo salvo!')};r.readAsDataURL(f)});

/* ===== NEW: Market selector in Settings ===== */
function openSettings(){
  $('#incPos').checked=INC_POS;
  $('#autoOpen').checked=AUTO_OPEN;
  renderMarketSelector();
  $('#settingsModal').classList.add('on');
}
function closeSettings(){$('#settingsModal').classList.remove('on')}

function renderMarketSelector(){
  var sel=$('#mktSelector');
  var grouped={};
  MARKET_GROUPS.forEach(function(g){grouped[g]=[]});
  Object.keys(MARKET_LIBRARY).forEach(function(k){var mkt=MARKET_LIBRARY[k];if(grouped[mkt.group]!==undefined)grouped[mkt.group].push(k)});
  
  var html='';
  MARKET_GROUPS.forEach(function(g){
    var keys=grouped[g];if(!keys.length)return;
    html+='<div class="mkt-group"><div class="mkt-group-h">'+escHtml(g)+'<div class="actions"><button onclick="mktGroupToggle(\''+g+'\',true)">Todos</button><button onclick="mktGroupToggle(\''+g+'\',false)">Nenhum</button></div></div><div class="mkt-list">';
    keys.forEach(function(k){
      var mkt=MARKET_LIBRARY[k];
      var isOn=ALWAYS_SHOW_KEYS.indexOf(k)>=0;
      html+='<label class="mkt-chk'+(isOn?' on':'')+'" data-mktchk="'+k+'"><input type="checkbox" '+(isOn?'checked':'')+' onchange="mktToggle(\''+k+'\',this.checked)"><span>'+escHtml(mkt.short)+'</span><span class="tagk">'+k+'</span></label>';
    });
    html+='</div></div>';
  });
  sel.innerHTML=html;
}

function mktToggle(key,checked){
  var idx=ALWAYS_SHOW_KEYS.indexOf(key);
  if(checked && idx<0)ALWAYS_SHOW_KEYS.push(key);
  else if(!checked && idx>=0)ALWAYS_SHOW_KEYS.splice(idx,1);
  var lbl=document.querySelector('[data-mktchk="'+key+'"]');
  if(lbl)lbl.classList.toggle('on',checked);
}

function mktGroupToggle(groupName,state){
  Object.keys(MARKET_LIBRARY).forEach(function(k){
    var mkt=MARKET_LIBRARY[k];
    if(mkt.group===groupName){
      mktToggle(k,state);
      var inp=document.querySelector('[data-mktchk="'+k+'"] input');
      if(inp)inp.checked=state;
    }
  });
}

function mktPreset(name){
  var preset=[];
  if(name==='default')preset=DEFAULT_ALWAYS_SHOW.slice();
  else if(name==='basic')preset=['home','draw','away'];
  else if(name==='goals')preset=['over15','under15','over25','under25','over35','under35','bttsYes','bttsNo'];
  else if(name==='all')preset=Object.keys(MARKET_LIBRARY);
  else if(name==='none')preset=[];
  ALWAYS_SHOW_KEYS=preset;
  renderMarketSelector();
}

function saveSettings(){
  INC_POS=$('#incPos').checked;
  AUTO_OPEN=$('#autoOpen').checked;
  localStorage.setItem('inc_pos',INC_POS);
  localStorage.setItem('auto_open',AUTO_OPEN);
  localStorage.setItem('always_show_keys',JSON.stringify(ALWAYS_SHOW_KEYS));
  closeSettings();
  if(AR.length)renderResults();
  toast('💾 Configurações salvas')
}

function parseNum(s){if(!s)return null;var m=String(s).replace(',','.').match(/-?[\d.]+/);return m?parseFloat(m[0]):null}
function parse(text,fileName){
  var d={fileName:fileName,home:'Time Casa',away:'Time Fora',date:'',time:'',comp:'',round:'',homeForm:'',awayForm:'',homeFmt:'',awayFmt:'',sampleSize:5,H:{},A:{},VL:[]};
  var keys=['ppg','winPct','firstToScore','avgScored','avgConceded','avgTotal','xgFor','xgAgainst','avgShots','avgShotsOnTarget','avgCorners','btts','over25','over15','cleanSheet','failedToScore','goalsScored1T','goalsConceded1T','won1T','fail1T','goalsScored2T','goalsConceded2T','won2T','fail2T'];
  keys.forEach(function(k){d.H[k]=null;d.A[k]=null});
  /* Preserve line structure from pdf.js extraction */
  var L=text.split(/\r?\n/).map(function(l){return l.replace(/\s+$/,'')}).filter(function(l){return l.length>0});
  function norm(s){return String(s||'').toLowerCase().normalize('NFD').replace(/[̀-ͯ]/g,'')}
  function findLine(label,after,opts){opts=opts||{};after=after||0;var nlabel=norm(label);for(var i=after;i<L.length;i++){var nl=norm(L[i]);if(nl.indexOf(nlabel)>=0){if(opts.excludeWords){var skip=false;opts.excludeWords.forEach(function(w){if(nl.indexOf(norm(w))>=0)skip=true});if(skip)continue}return i}}return -1}
  function getTwoNums(lineIdx){if(lineIdx<0)return [null,null];var ln=L[lineIdx];var nums=[];var re=/-?\d+(?:[,.]\d+)?%?/g,mm;while((mm=re.exec(ln))!==null){var s=mm[0].replace('%','').replace(',','.');var n=parseFloat(s);if(!isNaN(n))nums.push(n)}if(nums.length>=2)return [nums[0],nums[1]];if(nums.length===1)return [nums[0],null];return [null,null]}
  /* Smarter pair extractor: label + flex chars + number, ignores numbers inside the label */
  function getPair(lineIdx,labelText){
    if(lineIdx<0)return [null,null];
    var ln=L[lineIdx];
    var esc=labelText.replace(/[.*+?^${}()|[\]\\]/g,'\\$&');
    var re=new RegExp(esc+'[^\\d]{0,40}?(-?\\d+(?:[,.]\\d+)?)%?','gi');
    var found=[],m;
    while((m=re.exec(ln))!==null){
      var n=parseFloat(m[1].replace(',','.'));
      if(!isNaN(n))found.push(n);
    }
    if(found.length>=2)return [found[0],found[1]];
    if(found.length===1)return [found[0],null];
    return getTwoNums(lineIdx);
  }
  
  /* 1) Header line: "20/06/2026 19:00 Brazil Brazil Serie B Rodada - 14" */
  var headerLineIdx=-1;
  for(var i=0;i<L.length;i++){
    var m=L[i].match(/(\d{1,2}[\/\-]\d{1,2}[\/\-]\d{2,4})\s+(\d{1,2}:\d{2})(.*)$/);
    if(m){
      d.date=normalizeDate(m[1]);d.time=normalizeTime(m[2]);
      var rest=(m[3]||'').trim();
      var mr=rest.match(/(.*?)\s*Rodada\s*-?\s*(\d+)\s*$/i);
      if(mr){rest=mr[1].trim();d.round='Rodada '+mr[2]}
      else{var mr2=rest.match(/(.*?)\s*Round\s*-?\s*(\d+)\s*$/i);if(mr2){rest=mr2[1].trim();d.round='Rodada '+mr2[2]}}
      /* Dedupe repeated country prefix: "Brazil Brazil Serie B" -> "Brazil Serie B" */
      var compParts=rest.split(/\s+/);
      if(compParts.length>=2 && compParts[0]===compParts[1])compParts.shift();
      d.comp=compParts.join(' ').trim();
      headerLineIdx=i;break;
    }
  }
  
  /* 2) Team names — line right after header */
  if(headerLineIdx>=0){
    for(var j=headerLineIdx+1;j<Math.min(headerLineIdx+5,L.length);j++){
      var nl=L[j].trim();if(!nl)continue;
      /* Skip pure form lines (only W/D/L letters + spaces) */
      if(/^[WDL\s]+$/.test(nl) && nl.length<=40)continue;
      /* Try splitting by 2+ spaces (PDF column separator) */
      var parts=nl.split(/\s{2,}/);
      if(parts.length>=2){
        d.home=parts[0].trim();
        d.away=parts[parts.length-1].trim();
        break;
      }
      break;
    }
  }
  /* Fallback: parse from filename */
  if(d.home==='Time Casa' && fileName){
    var fn=fileName.replace(/\.pdf$/i,'').replace(/[_\-]+/g,' ');
    var mf=fn.match(/(.+?)\s+(?:vs?|x|×)\s+(.+)/i);
    if(mf){d.home=mf[1].trim();d.away=mf[2].trim()}
  }
  
  /* 3) Form line: "D W D L W L W W L D L L D L" — split in half */
  for(var i=0;i<L.length;i++){
    var ln=L[i].trim();
    var letterArr=ln.split(/\s+/).filter(function(c){return /^[WDL]$/.test(c)});
    if(letterArr.length>=2 && letterArr.length<=30){
      var compact=ln.replace(/\s+/g,'');
      if(/^[WDL]+$/.test(compact)){
        var half=Math.ceil(letterArr.length/2);
        var hForm=letterArr.slice(0,half).join('');
        var aForm=letterArr.slice(half).join('');
        d.homeForm=hForm.slice(-5);
        d.awayForm=aForm.slice(-5);
        break;
      }
    }
  }
  if(!d.homeForm)d.homeForm='';if(!d.awayForm)d.awayForm='';
  
  /* 4) Formations (tactical 4-4-2 etc) — optional, may not appear in Footystats */
  for(var i=0;i<L.length;i++){
    var fmtRe=/\b([3-5])\s*[-–]\s*([1-5])\s*[-–]\s*([1-5])(?:\s*[-–]\s*([1-5]))?\b/g;
    var mm,fmts=[];
    while((mm=fmtRe.exec(L[i]))!==null){
      var sum=parseInt(mm[1])+parseInt(mm[2])+parseInt(mm[3])+(mm[4]?parseInt(mm[4]):0);
      if(sum===10){fmts.push(mm[1]+'-'+mm[2]+'-'+mm[3]+(mm[4]?'-'+mm[4]:''))}
    }
    if(fmts.length>=1 && !d.homeFmt)d.homeFmt=fmts[0];
    if(fmts.length>=2 && !d.awayFmt)d.awayFmt=fmts[1];
    if(d.homeFmt && d.awayFmt)break;
  }
  
  /* 5) Core metric mappings — each label maps to [home, away] from same line */
  var mappings=[
    {label:'Pontos por Jogo',key:'ppg'},
    {label:'% de Vitórias',key:'winPct'},
    {label:'Primeiro a marcar',key:'firstToScore'},
    {label:'Ambas marcam',key:'btts'},
    {label:'Falhou em marcar',key:'failedToScore'},
    {label:'Jogos sem sofrer gols',key:'cleanSheet'},
    {label:'Média de gols marcados',key:'avgScored'},
    {label:'Média de gols sofridos',key:'avgConceded'},
    {label:'Média total de gols',key:'avgTotal'},
    {label:'Média de Finalizações no alvo',key:'avgShotsOnTarget'},
    {label:'xG médio a favor por jogo',key:'xgFor'},
    {label:'xG médio contra por jogo',key:'xgAgainst'},
    {label:'Média total de Cantos',key:'avgCorners'}
  ];
  mappings.forEach(function(map){
    var idx=findLine(map.label);
    if(idx>=0){var nums=getPair(idx,map.label);if(nums[0]!=null)d.H[map.key]=nums[0];if(nums[1]!=null)d.A[map.key]=nums[1]}
  });
  /* avgShots requires care: line "Média de Finalizações no alvo" matches both. Skip that for shots. */
  for(var i=0;i<L.length;i++){
    var nl=norm(L[i]);
    if(nl.indexOf(norm('Média de Finalizações'))>=0 && nl.indexOf(norm('no alvo'))<0){
      var nums=getPair(i,'Média de Finalizações');
      if(nums[0]!=null && d.H.avgShots==null)d.H.avgShots=nums[0];
      if(nums[1]!=null && d.A.avgShots==null)d.A.avgShots=nums[1];
      if(d.H.avgShots!=null && d.A.avgShots!=null)break;
    }
  }
  
  /* 6) Over 0.5/1.5/2.5 gols — first occurrence after "Total de Gols" header */
  var totGolsIdx=findLine('Total de Gols');
  if(totGolsIdx>=0){
    var o15=findLine('Over 1.5 gols',totGolsIdx);
    var o25=findLine('Over 2.5 gols',totGolsIdx);
    if(o15>=0){var n=getPair(o15,'Over 1.5 gols');if(n[0]!=null)d.H.over15=n[0];if(n[1]!=null)d.A.over15=n[1]}
    if(o25>=0){var n=getPair(o25,'Over 2.5 gols');if(n[0]!=null)d.H.over25=n[0];if(n[1]!=null)d.A.over25=n[1]}
  }
  
  /* 7) 1º / 2º Tempo sections */
  var perf1T=findLine('Desempenho no 1');
  if(perf1T>=0){
    var w1=findLine('Venceu o primeiro tempo',perf1T);
    var f1=findLine('Falhou em marcar',perf1T);
    var ms1=findLine('Média de gols marcados',perf1T);
    var mc1=findLine('Média de gols sofridos',perf1T);
    if(w1>=0){var n=getTwoNums(w1);d.H.won1T=n[0];d.A.won1T=n[1]}
    if(f1>=0){var n=getTwoNums(f1);d.H.fail1T=n[0];d.A.fail1T=n[1]}
    if(ms1>=0){var n=getTwoNums(ms1);d.H.goalsScored1T=n[0];d.A.goalsScored1T=n[1]}
    if(mc1>=0){var n=getTwoNums(mc1);d.H.goalsConceded1T=n[0];d.A.goalsConceded1T=n[1]}
  }
  var perf2T=findLine('Desempenho no 2');
  if(perf2T>=0){
    var w2=findLine('Venceu o segundo tempo',perf2T);
    var f2=findLine('Falhou em marcar',perf2T);
    var ms2=findLine('Média de gols marcados',perf2T);
    var mc2=findLine('Média de gols sofridos',perf2T);
    if(w2>=0){var n=getTwoNums(w2);d.H.won2T=n[0];d.A.won2T=n[1]}
    if(f2>=0){var n=getTwoNums(f2);d.H.fail2T=n[0];d.A.fail2T=n[1]}
    if(ms2>=0){var n=getTwoNums(ms2);d.H.goalsScored2T=n[0];d.A.goalsScored2T=n[1]}
    if(mc2>=0){var n=getTwoNums(mc2);d.H.goalsConceded2T=n[0];d.A.goalsConceded2T=n[1]}
  }
  
  /* 8) Sample size — "Total de Jogos N N" — use min of both teams */
  var totJ=findLine('Total de Jogos');
  if(totJ>=0){var nums=getTwoNums(totJ);if(nums[0]!=null){var s=nums[1]!=null?Math.min(nums[0],nums[1]):nums[0];d.sampleSize=Math.max(1,Math.round(s))}}
  
  /* 9) Validation — flag missing critical fields */
  if(d.H.ppg==null)d.VL.push({sev:'crit',msg:'PPG Casa não encontrado'});
  if(d.A.ppg==null)d.VL.push({sev:'crit',msg:'PPG Fora não encontrado'});
  if(d.H.avgScored==null)d.VL.push({sev:'warn',msg:'Média marcados Casa estimada'});
  if(d.A.avgScored==null)d.VL.push({sev:'warn',msg:'Média marcados Fora estimada'});
  if(d.sampleSize<5)d.VL.push({sev:'warn',msg:'Amostra pequena: '+d.sampleSize+' jogo(s) — score será penalizado em -10'});
  
  /* 10) Apply intelligent defaults for any remaining nulls */
  ['H','A'].forEach(function(t){
    var ref=(t==='H')?{scored:1.5,conceded:1.1}:{scored:1.2,conceded:1.4};
    if(d[t].avgScored==null)d[t].avgScored=ref.scored;
    if(d[t].avgConceded==null)d[t].avgConceded=ref.conceded;
    if(d[t].avgTotal==null)d[t].avgTotal=d[t].avgScored+d[t].avgConceded;
    if(d[t].xgFor==null)d[t].xgFor=d[t].avgScored*0.95;
    if(d[t].xgAgainst==null)d[t].xgAgainst=d[t].avgConceded*0.95;
    if(d[t].avgShots==null)d[t].avgShots=11;
    if(d[t].avgShotsOnTarget==null)d[t].avgShotsOnTarget=d[t].avgShots*0.36;
    if(d[t].avgCorners==null)d[t].avgCorners=5;
    if(d[t].ppg==null)d[t].ppg=(t==='H')?1.5:1.2;
    if(d[t].winPct==null)d[t].winPct=(t==='H')?48:36;
    if(d[t].firstToScore==null)d[t].firstToScore=(t==='H')?55:45;
    if(d[t].btts==null)d[t].btts=52;
    if(d[t].over25==null)d[t].over25=52;
    if(d[t].over15==null)d[t].over15=75;
    if(d[t].cleanSheet==null)d[t].cleanSheet=28;
    if(d[t].failedToScore==null)d[t].failedToScore=28;
    if(d[t].goalsScored2T==null)d[t].goalsScored2T=d[t].avgScored*0.6;
    if(d[t].goalsConceded2T==null)d[t].goalsConceded2T=d[t].avgConceded*0.6;
    if(d[t].won2T==null)d[t].won2T=30;
    if(d[t].fail2T==null)d[t].fail2T=30;
    if(d[t].goalsScored1T==null)d[t].goalsScored1T=d[t].avgScored*0.4;
    if(d[t].goalsConceded1T==null)d[t].goalsConceded1T=d[t].avgConceded*0.4;
  });
  if(!d.homeForm)d.homeForm='WDLWL';if(!d.awayForm)d.awayForm='LWDLW';
  
  /* 11) Apply date/time overrides from localStorage */
  var k=matchKey(d);
  if(DT_OVERRIDES[k]){if(DT_OVERRIDES[k].date)d.date=DT_OVERRIDES[k].date;if(DT_OVERRIDES[k].time)d.time=DT_OVERRIDES[k].time}
  
  return d;
}
function buildPoisson(d){var lh=(d.H.avgScored+d.A.avgConceded)/2,la=(d.A.avgScored+d.H.avgConceded)/2;lh=Math.max(0.2,Math.min(4.5,lh));la=Math.max(0.2,Math.min(4.5,la));var matrix=[],pHome=0,pDraw=0,pAway=0,scores=[];for(var i=0;i<6;i++){matrix[i]=[];for(var j=0;j<6;j++){var p=poissonPMF(lh,i)*poissonPMF(la,j);matrix[i][j]=p;scores.push({i:i,j:j,p:p});if(i>j)pHome+=p;else if(i<j)pAway+=p;else pDraw+=p}}scores.sort(function(a,b){return b.p-a.p});return {lh:lh,la:la,matrix:matrix,pHome:pHome,pDraw:pDraw,pAway:pAway,topScores:scores.slice(0,5).map(function(s){return {score:s.i+'-'+s.j,p:s.p}})}}
function effMomentum(t){return {atkVsXg:t.xgFor>0?t.avgScored/t.xgFor:1,defVsXg:t.xgAgainst>0?t.avgConceded/t.xgAgainst:1,conv:t.avgShots>0?(t.avgScored/t.avgShots)*100:0,sotPct:t.avgShots>0?(t.avgShotsOnTarget/t.avgShots)*100:0}}
function styleTag(t){if(t.avgScored>=2.0&&t.avgConceded<=1.0)return 'DOMINANTE';if(t.avgScored>=1.8&&t.avgConceded>=1.5)return 'OFENSIVO';if(t.avgScored<=1.2&&t.avgConceded<=0.9)return 'DEFENSIVO';if(t.avgScored<=1.0&&t.avgConceded>=1.5)return 'FRACO';return 'EQUILIBRADO'}
function formMomentum(form){if(!form)return 50;var w=0,e=0,g=form.length;for(var i=0;i<form.length;i++){if(form[i]==='W')w++;else if(form[i]==='D')e++}return g>0?((w*3+e*1)/(g*3))*100:50}
function aM(id,name,type,criteria,risk,market,entry,exit,stake,oddMin,oddMax,oddIdeal,highValue){return {id:id,name:name,type:type,criteria:criteria,risk:risk,market:market,entry:entry,exit:exit,stake:stake,oddMin:oddMin,oddMax:oddMax,oddIdeal:oddIdeal,highValue:highValue,score:0,verdict:'REJEITADA'}}
function C(label,ok,w,raw){return {label:label,ok:!!ok,w:w||1,raw:raw||''}}
function gV(m){if(m.score>=70)return 'CONFIRMADA';if(m.score>=50)return 'POSSÍVEL';if(m.score>=30)return 'EVITAR';return 'REJEITADA'}

function getProbSuccess(r,m){var P=r.a.poisson,d=r.d,a=r.a;switch(m.id){case 1:case 10:case 13:return a.fav==='home'?P.pHome:P.pAway;case 2:case 11:{var p=0;for(var i=1;i<6;i++)for(var j=1;j<6;j++)if(i+j>=3)p+=P.matrix[i][j];return p}case 3:{var p=0;for(var i=0;i<6;i++)for(var j=0;j<6;j++)if(i+j>=4)p+=P.matrix[i][j];return p}case 4:{var avg2T=(d.H.goalsScored2T+d.A.goalsScored2T)/2;return 1-Math.exp(-(avg2T*(20/45)))}case 5:return 1-P.matrix[1][0];case 6:return 1-P.matrix[0][1];case 9:return 1-(a.ppgDiff>=0?P.pAway:P.pHome);case 12:return 1-P.pDraw;}return 0.5}
function getMethodPrimaryMarketKey(m,r){var keys=METHOD_MARKETS[m.id];if(!keys||!keys.length)return null;if(m.id===1||m.id===10||m.id===13)return r.a.fav==='home'?keys[0]:keys[1];if(m.id===9)return r.a.fav==='home'?'layAway':'layHome';return keys[0]}
function getMatchOdd(d,k){var mk=matchKey(d);return MATCH_ODDS[mk]?MATCH_ODDS[mk][k]||null:null}
function setMatchOdd(d,k,v){var mk=matchKey(d);if(!MATCH_ODDS[mk])MATCH_ODDS[mk]={};if(v&&v>=1.01){MATCH_ODDS[mk][k]=v}else{delete MATCH_ODDS[mk][k]}localStorage.setItem('match_odds',JSON.stringify(MATCH_ODDS))}
function getRealProbForMarket(r,k){var P=r.a.poisson,d=r.d;switch(k){case 'home':return P.pHome;case 'draw':return P.pDraw;case 'away':return P.pAway;case 'over15':{var p=0;for(var i=0;i<6;i++)for(var j=0;j<6;j++)if(i+j>=2)p+=P.matrix[i][j];return p}case 'under15':{var p=0;for(var i=0;i<6;i++)for(var j=0;j<6;j++)if(i+j<2)p+=P.matrix[i][j];return p}case 'over25':{var p=0;for(var i=0;i<6;i++)for(var j=0;j<6;j++)if(i+j>=3)p+=P.matrix[i][j];return p}case 'under25':{var p=0;for(var i=0;i<6;i++)for(var j=0;j<6;j++)if(i+j<3)p+=P.matrix[i][j];return p}case 'over35':{var p=0;for(var i=0;i<6;i++)for(var j=0;j<6;j++)if(i+j>=4)p+=P.matrix[i][j];return p}case 'under35':{var p=0;for(var i=0;i<6;i++)for(var j=0;j<6;j++)if(i+j<4)p+=P.matrix[i][j];return p}case 'bttsYes':{var p=0;for(var i=1;i<6;i++)for(var j=1;j<6;j++)p+=P.matrix[i][j];return p}case 'bttsNo':{var p=0;for(var i=0;i<6;i++)for(var j=0;j<6;j++)if(i===0||j===0)p+=P.matrix[i][j];return p}case 'layCS10':return 1-P.matrix[1][0];case 'layCS01':return 1-P.matrix[0][1];case 'layCS20':return 1-P.matrix[2][0];case 'layCS02':return 1-P.matrix[0][2];case 'layDraw':return 1-P.pDraw;case 'layHome':return 1-P.pHome;case 'layAway':return 1-P.pAway;case 'ah05Home':return P.pHome;case 'ah05Away':return P.pAway;case 'over05_2T':{var avg2T=(d.H.goalsScored2T+d.A.goalsScored2T)/2;return 1-Math.exp(-(avg2T*(20/45)))}}return null}
function calcMarketEV(r,k,od){if(!od||od<1.01)return null;var p=getRealProbForMarket(r,k);if(p==null)return null;return {p:p,ev:p*od-1,impProb:1/od}}
function analyzeOdd(m,odd,r){if(isNaN(odd)||odd<1.01)return null;var ps=getProbSuccess(r,m);var ev=ps*odd-1;if(m.type==='lay'&&odd>20)return {status:'wait',cls:'wait',icon:'⏳',label:'AGUARDE LIVE — Odd muito alta',msg:'Essa odd de Lay está alta. Espere o jogo começar.',ps:ps,ev:ev};if(odd<m.oddMin)return {status:'low',cls:'no',icon:'⛔',label:'NÃO ENTRAR — Odd muito baixa',msg:'Odd '+odd.toFixed(2)+' < mínimo '+m.oddMin.toFixed(2)+'.',ps:ps,ev:ev};if(odd>m.oddMax&&m.oddMax<99)return {status:'high',cls:'warn',icon:'⚠️',label:'CUIDADO — Odd acima do máximo',msg:'Odd '+odd.toFixed(2)+' > máx '+m.oddMax.toFixed(2)+'.',ps:ps,ev:ev};if(m.highValue&&odd>=m.oddIdeal)return {status:'hv',cls:'hv',icon:'🌟',label:'⭐ ALTO VALOR',msg:'Odd '+odd.toFixed(2)+' acima do ideal '+m.oddIdeal.toFixed(2)+'.',ps:ps,ev:ev};if(Math.abs(odd-m.oddIdeal)<=0.20)return {status:'ideal',cls:'execute',icon:'✅',label:'EXECUTAR — Odd ideal',msg:'Odd '+odd.toFixed(2)+' na faixa ideal.',ps:ps,ev:ev};return {status:'ok',cls:'execute',icon:'✅',label:'EXECUTAR',msg:'Odd '+odd.toFixed(2)+' dentro da faixa.',ps:ps,ev:ev}}
function getEntryFinalStatus(r,m){var k=getMethodPrimaryMarketKey(m,r);if(!k)return null;var o=getMatchOdd(r.d,k);if(!o)return null;return analyzeOdd(m,o,r)}
function statusRank(st){if(!st)return 5;if(st.cls==='hv')return 0;if(st.cls==='execute')return 1;if(st.cls==='wait')return 3;if(st.cls==='warn')return 4;return 6}

function analyze(d){
  var P=buildPoisson(d);var H=d.H,A=d.A;
  var effH=effMomentum(H),effA=effMomentum(A);
  var styH=styleTag(H),styA=styleTag(A);
  var momH=formMomentum(d.homeForm),momA=formMomentum(d.awayForm);
  var ppgDiff=H.ppg-A.ppg;var fav=ppgDiff>=0?'home':'away';
  var favName=fav==='home'?d.home:d.away,udName=fav==='home'?d.away:d.home;
  var F=fav==='home'?H:A,U=fav==='home'?A:H;
  var avgTotal=(H.avgScored+A.avgScored+H.avgConceded+A.avgConceded)/2;
  var bttsAvg=(H.btts+A.btts)/2,o25Avg=(H.over25+A.over25)/2,xgComb=H.xgFor+A.xgFor;
  var formH3W=(d.homeForm.split('W').length-1)>=3,formA3W=(d.awayForm.split('W').length-1)>=3;
  var totalCorners=H.avgCorners+A.avgCorners,pDraw100=P.pDraw*100;
  var methods=[];
  methods.push(aM(1,'Back Favorito','back',[C('Diferença PPG ≥1.2',Math.abs(ppgDiff)>=1.2,3,fmt(Math.abs(ppgDiff),2)),C('Win% favorito >55%',F.winPct>55,2,fmt(F.winPct,0)+'%'),C('1º a marcar favorito >60%',F.firstToScore>60,2,fmt(F.firstToScore,0)+'%'),C('Forma favorito ≥3W em 5',fav==='home'?formH3W:formA3W,3,fav==='home'?d.homeForm:d.awayForm),C('Gols/jogo favorito ≥1.5',F.avgScored>=1.5,2,fmt(F.avgScored,2)),C('xG_for > xG_against',F.xgFor>F.xgAgainst,1,''),C('Clean Sheet favorito >30%',F.cleanSheet>30,1,fmt(F.cleanSheet,0)+'%')],'BAIXO','Vitória '+favName,'Pré-jogo','Cash out após 1º gol contra','1-2%',1.55,2.20,1.85,false));
  methods.push(aM(2,'Back 2×2 (BTTS Sim + Over 2.5)','back',[C('BTTS médio >55%',bttsAvg>55,3,fmt(bttsAvg,0)+'%'),C('Over 2.5 médio >55%',o25Avg>55,3,fmt(o25Avg,0)+'%'),C('Média gols ≥2.8',avgTotal>=2.8,3,fmt(avgTotal,2)),C('Failed to score <25% (ambos)',H.failedToScore<25&&A.failedToScore<25,2,''),C('xG combinado ≥2.5',xgComb>=2.5,2,fmt(xgComb,2))],'MEDIO','BTTS SIM + Over 2.5','Pré-jogo','Cash out se 1-1 no 70min','2%',1.90,9999,2.80,true));
  methods.push(aM(3,'Back Goleada (Over 3.5)','back',[C('PPG diff ≥2.0',Math.abs(ppgDiff)>=2.0,3,fmt(Math.abs(ppgDiff),2)),C('Favorito gols/jogo ≥2.2',F.avgScored>=2.2,3,fmt(F.avgScored,2)),C('Adversário sofre ≥2',U.avgConceded>=2.0,2,fmt(U.avgConceded,2)),C('Adversário falhou marcar ≥40%',U.failedToScore>=40,2,fmt(U.failedToScore,0)+'%')],'ALTO','Over 3.5','Pré-jogo','Cash out se 1 gol no 70min','1%',2.30,9999,3.50,true));
  var avg2T=(H.goalsScored2T+A.goalsScored2T)/2;
  methods.push(aM(4,'Over 70min (Live)','back',[C('Média gols 2T ≥1.3',avg2T>=1.3,3,fmt(avg2T,2)),C('1º a marcar 2T >35%',(H.won2T+A.won2T)/2>35,2,''),C('PPG diff <1 (equilibrado)',Math.abs(ppgDiff)<1,2,fmt(Math.abs(ppgDiff),2)),C('Gols totais 2T estimado ≥10',avg2T*5>=10,1,'')],'BAIXO','Over 0.5 entre 70-90min','LIVE 68-72min','Sair após 1º gol','2%',1.45,2.00,1.70,false));
  function pCS(i,j){return P.matrix[i][j]*100}
  methods.push(aM(5,'Lay 1×0','lay',[C('P(1-0) <12%',pCS(1,0)<12,3,fmt(pCS(1,0),1)+'%'),C('Visitante Over 0.5 >65%',A.over15>65,2,''),C('Visitante marca >60%',(100-A.failedToScore)>60,2,''),C('Média gols ≥2.5',avgTotal>=2.5,2,fmt(avgTotal,2)),C('BTTS médio >50%',bttsAvg>50,1,'')],'MEDIO','Lay 1-0','Pré-jogo','Cash out se 0-1 ou 1-1','2%',9.0,15.0,12.0,false));
  methods.push(aM(6,'Lay 0×1','lay',[C('P(0-1) <12%',pCS(0,1)<12,3,fmt(pCS(0,1),1)+'%'),C('Mandante Over 0.5 >65%',H.over15>65,2,''),C('Mandante marca >60%',(100-H.failedToScore)>60,2,''),C('Média gols ≥2.5',avgTotal>=2.5,2,fmt(avgTotal,2)),C('BTTS médio >50%',bttsAvg>50,1,'')],'MEDIO','Lay 0-1','Pré-jogo','Cash out se 1-0 ou 1-1','2%',9.0,15.0,12.0,false));
  var azarao=ppgDiff>=0?'A':'H',Z=azarao==='H'?H:A,pZebra=ppgDiff>=0?P.pAway*100:P.pHome*100;
  methods.push(aM(9,'Lay Zebra','lay',[C('Diferença força ≥1.5',Math.abs(ppgDiff)>=1.5,3,fmt(Math.abs(ppgDiff),2)),C('Azarão win% <30%',Z.winPct<30,3,fmt(Z.winPct,0)+'%'),C('Favorito 3+W em 5',ppgDiff>=0?formH3W:formA3W,2,''),C('Azarão gols/jogo <1',Z.avgScored<1,1,fmt(Z.avgScored,2)),C('P(azarão) <20%',pZebra<20,2,fmt(pZebra,1)+'%')],'MEDIO','Lay azarão ('+udName+')','Pré-jogo','Cash out se azarão marcar','2%',3.50,6.00,4.50,false));
  var Feff=fav==='home'?effH:effA,Fmom=fav==='home'?momH:momA,Fsty=fav==='home'?styH:styA;
  methods.push(aM(10,'Momentum & Eficiência (Vitória)','back',[C('Momentum favorito ≥60%',Fmom>=60,3,fmt(Fmom,0)+'%'),C('Estilo DOMINANTE/OFENSIVO',Fsty==='DOMINANTE'||Fsty==='OFENSIVO',3,Fsty),C('Ataque vs xG ≥1.0',Feff.atkVsXg>=1.0,2,fmt(Feff.atkVsXg,2)+'×'),C('Defesa vs xG ≤1.0',Feff.defVsXg<=1.0,2,fmt(Feff.defVsXg,2)+'×'),C('Forma ≥3W/5',fav==='home'?formH3W:formA3W,2,''),C('Conversão ≥10%',Feff.conv>=10,1,fmt(Feff.conv,1)+'%')],'BAIXO','Vitória '+favName,'Pré-jogo','Cash out após 1º gol contra','2%',1.50,2.30,1.85,false));
  var atkAvg=(effH.atkVsXg+effA.atkVsXg)/2,convAvg=(effH.conv+effA.conv)/2,failAvg=(H.failedToScore+A.failedToScore)/2,momAvg=(momH+momA)/2;
  var anyOff=['DOMINANTE','OFENSIVO'].indexOf(styH)>=0||['DOMINANTE','OFENSIVO'].indexOf(styA)>=0;
  methods.push(aM(11,'Momentum & Eficiência (Gols)','back',[C('Over 2.5 médio ≥55%',o25Avg>=55,3,fmt(o25Avg,0)+'%'),C('BTTS médio ≥55%',bttsAvg>=55,3,fmt(bttsAvg,0)+'%'),C('Média gols ≥2.7',avgTotal>=2.7,2,fmt(avgTotal,2)),C('xG combinado ≥2.5',xgComb>=2.5,2,fmt(xgComb,2)),C('Ataque vs xG médio ≥0.95',atkAvg>=0.95,1,fmt(atkAvg,2)+'×'),C('Conversão média ≥10%',convAvg>=10,1,fmt(convAvg,1)+'%'),C('Falhou marcar <30% médio',failAvg<30,1,fmt(failAvg,0)+'%'),C('Momentum médio ≥50%',momAvg>=50,2,fmt(momAvg,0)+'%'),C('1+ time DOM/OFENSIVO',anyOff,1,'')],'MEDIO','Over 2.5 + BTTS SIM','Pré-jogo','Cash out se 0-0 no 70min','2%',1.80,9999,2.80,true));
  methods.push(aM(12,'Lay Empate','lay',[C('P(empate) <25%',pDraw100<25,3,fmt(pDraw100,1)+'%'),C('Média gols ≥2.8',avgTotal>=2.8,3,fmt(avgTotal,2)),C('BTTS médio >55%',bttsAvg>55,2,fmt(bttsAvg,0)+'%'),C('Diff força clara (|PPG|≥0.8)',Math.abs(ppgDiff)>=0.8,2,''),C('Pelo menos 1 time ofensivo',anyOff,2,'')],'MEDIO','Lay Empate','Pré-jogo','Cash out após 1º gol','2%',3.0,4.5,3.5,false));
  methods.push(aM(13,'Asian Handicap -0.5','back',[C('PPG diff ≥1.5',Math.abs(ppgDiff)>=1.5,3,fmt(Math.abs(ppgDiff),2)),C('Favorito é mandante',fav==='home',2,fav==='home'?'SIM':'NÃO'),C('Momentum favorito ≥55%',Fmom>=55,2,fmt(Fmom,0)+'%'),C('Conversão favorito ≥12%',Feff.conv>=12,2,fmt(Feff.conv,1)+'%'),C('xG favorito > adversário',F.xgFor>U.xgFor,1,''),C('Forma ≥3W em 5',fav==='home'?formH3W:formA3W,2,'')],'BAIXO','AH -0.5 '+favName,'Pré-jogo','Cash out após 1º gol contra','2%',1.70,2.30,1.95,false));
  methods.forEach(function(m){var totalW=0,achievedW=0;m.criteria.forEach(function(c){totalW+=(c.w||1);if(c.ok)achievedW+=(c.w||1)});m.score=totalW>0?Math.round((achievedW/totalW)*100):0;if(d.sampleSize<5)m.score=Math.max(0,m.score-10);m.verdict=gV(m)});
  return {methods:methods,poisson:P,eff:{home:effH,away:effA},style:{home:styH,away:styA},momentum:{home:momH,away:momA},fav:fav,favName:favName,udName:udName,ppgDiff:ppgDiff,avgTotal:avgTotal,bttsAvg:bttsAvg,o25Avg:o25Avg,xgComb:xgComb,totalCorners:totalCorners};
}

async function processAll(){
  if(!FILES.length){toast('Adicione PDFs primeiro',true);return}
  $('#procBtn').disabled=true;$('#procBtn').innerHTML='⏳ Processando...';
  AR=[];
  for(var i=0;i<FILES.length;i++){
    try{
      var f=FILES[i];var ab=await f.arrayBuffer();var pdf=await pdfjsLib.getDocument({data:ab}).promise;var allText='';
      for(var p=1;p<=pdf.numPages;p++){
        var page=await pdf.getPage(p);var tc=await page.getTextContent();
        /* Group items by Y coordinate to preserve line structure */
        var byLine={};
        tc.items.forEach(function(it){
          if(!it.transform)return;
          var y=Math.round(it.transform[5]);
          if(!byLine[y])byLine[y]=[];
          byLine[y].push({x:it.transform[4],s:it.str});
        });
        var sortedYs=Object.keys(byLine).map(Number).sort(function(a,b){return b-a});
        var pageText=sortedYs.map(function(y){
          var items=byLine[y].sort(function(a,b){return a.x-b.x});
          return items.map(function(it){return it.s}).join(' ');
        }).join('\n');
        allText+=pageText+'\n';
      }
      var d=parse(allText,f.name);var a=analyze(d);AR.push({d:d,a:a,file:f.name});
    }catch(err){console.error('Erro',FILES[i].name,err);toast('Erro em '+FILES[i].name,true)}
  }
  $('#procBtn').disabled=false;$('#procBtn').innerHTML='⚙ Processar Análise';
  if(AR.length){$('#emptyState').style.display='none';renderResults();toast('✓ '+AR.length+' partida(s) processada(s)!')}
  else{toast('Nenhuma partida processada',true)}
}

function renderResults(){
  $('#results').style.display='block';
  renderSideMenu();
  renderFlatGrid();
  renderMatchList();
  applyViewToggle();
}
function setView(v){
  CURRENT_VIEW=v;localStorage.setItem('current_view',v);
  applyViewToggle();
}
function applyViewToggle(){
  document.querySelectorAll('.view-tab').forEach(function(t){t.classList.toggle('on',t.dataset.view===CURRENT_VIEW)});
  $('#viewGrid').style.display=CURRENT_VIEW==='grid'?'grid':'none';
  $('#viewMatch').style.display=CURRENT_VIEW==='match'?'block':'none';
}

/* === Flat Grid View === */
function getAllEntriesFlat(){
  var out=[];
  AR.forEach(function(r,matchIdx){
    var entries=r.a.methods.filter(function(m){return m.verdict==='CONFIRMADA'||(INC_POS&&m.verdict==='POSSÍVEL')});
    entries.forEach(function(m){out.push({r:r,m:m,matchIdx:matchIdx})});
  });
  return out;
}
function filterFlatEntries(entries){
  return entries.filter(function(e){
    if(METHOD_FILTER.indexOf(e.m.id)<0)return false;
    if(ONLY_HV && !e.m.highValue)return false;
    if(ONLY_APPROVED){var st=getEntryFinalStatus(e.r,e.m);if(!st||(st.cls!=='execute'&&st.cls!=='hv'))return false}
    return true;
  });
}
function sortFlatEntries(entries){
  return entries.sort(function(a,b){
    var stA=getEntryFinalStatus(a.r,a.m),stB=getEntryFinalStatus(b.r,b.m);
    var rA=statusRank(stA),rB=statusRank(stB);
    if(rA!==rB)return rA-rB;
    var pA=getProbSuccess(a.r,a.m),pB=getProbSuccess(b.r,b.m);
    if(Math.abs(pA-pB)>0.03)return pB-pA;
    return b.m.score-a.m.score;
  });
}
function renderSideMenu(){
  var entries=getAllEntriesFlat();
  var counts={};
  entries.forEach(function(e){counts[e.m.id]=(counts[e.m.id]||0)+1});
  var allMethodIds=[1,2,3,4,5,6,9,10,11,12,13];
  var html='<div class="side-h">🎯 Filtros</div>';
  html+='<div class="side-quick"><button onclick="filterAll(true)">Todos</button><button onclick="filterAll(false)">Nenhum</button></div>';
  html+='<div class="side-section-h">Métodos</div><div class="side-methods">';
  allMethodIds.forEach(function(id){
    var L=METHOD_LEGENDS[id];if(!L)return;
    var count=counts[id]||0;
    var checked=METHOD_FILTER.indexOf(id)>=0;
    html+='<label class="side-method-chk'+(checked?' on':'')+(count===0?' empty':'')+'">';
    html+='<input type="checkbox" '+(checked?'checked':'')+' onchange="toggleMethodFilter('+id+',this.checked)">';
    html+='<span class="side-method-num">'+L.label+'</span>';
    html+='<span class="side-method-title">'+L.icon+' '+escHtml(L.title.split('(')[0].trim())+'</span>';
    html+='<span class="side-method-count">'+count+'</span>';
    html+='</label>';
  });
  html+='</div>';
  html+='<div class="side-section-h">Filtros rápidos</div><div class="side-extra">';
  html+='<label class="chk-lbl"><input type="checkbox"'+(ONLY_HV?' checked':'')+' onchange="setOnlyHV(this.checked)"> ⭐ Apenas HV</label>';
  html+='<label class="chk-lbl"><input type="checkbox"'+(ONLY_APPROVED?' checked':'')+' onchange="setOnlyApproved(this.checked)"> ✅ Apenas Aprovadas</label>';
  html+='<label class="chk-lbl"><input type="checkbox"'+(INC_POS?' checked':'')+' onchange="setIncPosSide(this.checked)"> Incluir POSSÍVEIS</label>';
  html+='</div>';
  $('#sideMenu').innerHTML=html;
}
function toggleMethodFilter(id,checked){
  var idx=METHOD_FILTER.indexOf(id);
  if(checked && idx<0)METHOD_FILTER.push(id);
  else if(!checked && idx>=0)METHOD_FILTER.splice(idx,1);
  localStorage.setItem('method_filter',JSON.stringify(METHOD_FILTER));
  renderSideMenu();renderFlatGrid();
}
function filterAll(state){
  METHOD_FILTER=state?[1,2,3,4,5,6,9,10,11,12,13]:[];
  localStorage.setItem('method_filter',JSON.stringify(METHOD_FILTER));
  renderSideMenu();renderFlatGrid();
}
function setOnlyHV(v){ONLY_HV=v;localStorage.setItem('only_hv',v);renderFlatGrid()}
function setOnlyApproved(v){ONLY_APPROVED=v;localStorage.setItem('only_approved',v);renderFlatGrid()}
function setIncPosSide(v){INC_POS=v;localStorage.setItem('inc_pos',v);renderSideMenu();renderFlatGrid();renderMatchList()}
function renderFlatGrid(){
  var entries=sortFlatEntries(filterFlatEntries(getAllEntriesFlat()));
  var grid=$('#flatGrid');
  if(!entries.length){
    grid.innerHTML='<div class="empty" style="grid-column:1/-1"><div class="empty-ic">🔎</div><h3>Nenhuma entrada</h3><p>Marque pelo menos um método no menu lateral ou ative POSSÍVEIS.</p></div>';
    updateGlobalCounts();return;
  }
  grid.innerHTML=entries.map(function(e,gi){return renderEntryCard(e.r,e.m,e.matchIdx,gi)}).join('');
  updateGlobalCounts();
}
function renderEntryCard(r,m,matchIdx,gridIdx){
  var L=METHOD_LEGENDS[m.id];var d=r.d;
  var ps=getProbSuccess(r,m);var psPct=Math.round(ps*100);
  var psClass=psPct>=70?'high':psPct>=50?'mid':'low';
  var finalSt=getEntryFinalStatus(r,m);
  var statusCls=finalSt?' s-'+finalSt.cls:'';
  var hvCls=m.highValue?' hv':'';
  var rankMedal=gridIdx<=2?'<span class="ecard-rank r'+(gridIdx+1)+'">'+(gridIdx+1)+'</span>':'';
  var headStatus=finalSt
    ?'<span class="ecard-status ehs-'+finalSt.cls+'">'+finalSt.icon+' '+escHtml(finalSt.label.split('—')[0].trim())+'</span>'
    :'<span class="ecard-status empty">⏳ aguardando odd</span>';
  var status=timeStatus(d.date,d.time);
  var dtPills=status==='live'?'<span class="ecard-live">🔴 AO VIVO</span>':status==='soon'?'<span class="ecard-soon">⏳ EM BREVE</span>':'';
  return '<div class="ecard'+hvCls+statusCls+'" data-eid="'+matchIdx+'-'+m.id+'">'+
    rankMedal+
    '<div class="ecard-match"><div class="ecard-match-top">'+
      '<span class="ecard-match-teams">'+escHtml(d.home)+' <span class="vs">×</span> '+escHtml(d.away)+'</span>'+
      dtPills+
    '</div><div class="ecard-match-bot">'+
      '<span class="ecard-match-dt">📅 '+escHtml(d.date||'?')+(d.time?' • '+escHtml(d.time):'')+'</span>'+
      (d.comp?'<span class="ecard-match-comp">🏆 '+escHtml(d.comp)+'</span>':'')+
    '</div></div>'+
    '<div class="ecard-method">'+
      '<div class="ecard-method-num">'+L.label+'</div>'+
      '<div class="ecard-method-text">'+
        '<div class="ecard-method-title">'+escHtml(L.title)+(m.highValue?' <span class="star">★</span>':'')+'</div>'+
        '<div class="ecard-method-mkt"><span class="tag '+m.type+'">'+m.type.toUpperCase()+'</span><span>📍 '+escHtml(m.market)+'</span></div>'+
      '</div>'+
    '</div>'+
    '<div class="ecard-stats">'+
      '<div class="ecard-stat"><span class="lbl">Score</span><span class="v score-v">'+m.score+'/100</span></div>'+
      '<div class="ecard-stat"><span class="lbl">Chance</span><span class="v prob '+psClass+'">'+psPct+'%</span></div>'+
    '</div>'+
    '<div class="ecard-status-row">'+headStatus+'</div>'+
    '<button class="ecard-expand-btn" onclick="toggleEntryCard('+matchIdx+','+m.id+')">▼ Expandir detalhes</button>'+
    '<div class="ecard-body" id="ecard-body-'+matchIdx+'-'+m.id+'" style="display:none">'+
      renderEntryCardBody(r,m,matchIdx)+
    '</div>'+
  '</div>';
}
function renderEntryCardBody(r,m,matchIdx){
  var L=METHOD_LEGENDS[m.id];var d=r.d;
  var ps=getProbSuccess(r,m);
  var motivo=buildMotivoInformal(r,m,L,ps);
  var critHtml=m.criteria.map(function(c){
    var ic=c.ok?'✓':'✗',cls=c.ok?'ok':'no';
    var w=c.w>1?'<span class="w">peso '+c.w+'</span>':'';
    var raw=c.raw?' <span style="color:var(--text-tertiary);font-family:JetBrains Mono;font-size:11px">('+escHtml(c.raw)+')</span>':'';
    return '<li class="'+cls+'"><span class="ic">'+ic+'</span><span class="txt">'+escHtml(c.label)+raw+'</span>'+w+'</li>';
  }).join('');
  var oddMaxTxt=m.oddMax>=99?'∞':m.oddMax.toFixed(2);
  var statusPanel=buildEntryStatusPanel(r,m,matchIdx);
  var primaryKey=getMethodPrimaryMarketKey(m,r);
  var oddInput='';
  if(primaryKey){
    var mkt=MARKET_LIBRARY[primaryKey];
    var savedOdd=getMatchOdd(d,primaryKey)||'';
    oddInput='<div class="ecard-odd-input">'+
      '<label>💰 '+escHtml(mkt.short)+'</label>'+
      '<input type="number" step="0.01" min="1.01" placeholder="0.00" value="'+escHtml(savedOdd)+'" oninput="onCardOddInput('+matchIdx+',\''+primaryKey+'\',this.value,'+m.id+')">'+
      '<button class="btn alt sm" onclick="openPasteModal('+matchIdx+')">📋 Colar várias</button>'+
    '</div>';
  }
  return '<div class="section"><div class="section-h">💡 Por que essa entrada?</div><div class="motivo">'+motivo+'</div></div>'+
    (oddInput?'<div class="section"><div class="section-h gold">💰 Sua odd</div>'+oddInput+'</div>':'')+
    '<div class="section"><div class="section-h gold">🎯 Decisão final</div>'+statusPanel+'</div>'+
    '<div class="section"><div class="section-h">📋 Requisitos</div><div class="req-grid">'+
      '<div class="req-box entrada"><h5>🟢 Entrada</h5><ul>'+
        '<li><span class="k">Mercado</span><span class="v">'+escHtml(m.market)+'</span></li>'+
        '<li><span class="k">Tipo</span><span class="v">'+m.type.toUpperCase()+'</span></li>'+
        '<li><span class="k">Momento</span><span class="v">'+escHtml(m.entry)+'</span></li>'+
        '<li><span class="k">Stake</span><span class="v">'+m.stake+'</span></li>'+
        '<li><span class="k">Risco</span><span class="v">'+m.risk+'</span></li>'+
      '</ul><div class="odds-box">'+
        '<div class="ob"><div class="ol">Mín</div><div class="ov">'+m.oddMin.toFixed(2)+'</div></div>'+
        '<div class="ob ideal"><div class="ol">Ideal</div><div class="ov">'+m.oddIdeal.toFixed(2)+'</div></div>'+
        '<div class="ob"><div class="ol">Máx</div><div class="ov">'+oddMaxTxt+'</div></div>'+
      '</div></div>'+
      '<div class="req-box saida"><h5>🟡 Saída</h5><ul>'+
        '<li><span class="k">Gatilho</span><span class="v" style="font-size:12px;text-align:right;line-height:1.4">'+escHtml(m.exit)+'</span></li>'+
        '<li><span class="k">Veredicto</span><span class="v" style="color:var(--success)">'+m.verdict+'</span></li>'+
      '</ul></div>'+
    '</div></div>'+
    '<div class="section"><div class="section-h suc">✓ Critérios ('+m.criteria.filter(function(c){return c.ok}).length+'/'+m.criteria.length+')</div>'+
      '<ul class="crit">'+critHtml+'</ul>'+
    '</div>';
}
function toggleEntryCard(matchIdx,mid){
  var body=document.getElementById('ecard-body-'+matchIdx+'-'+mid);
  var card=document.querySelector('.ecard[data-eid="'+matchIdx+'-'+mid+'"]');
  if(!body||!card)return;
  var isOpen=body.style.display!=='none';
  body.style.display=isOpen?'none':'block';
  card.classList.toggle('open',!isOpen);
  var btn=card.querySelector('.ecard-expand-btn');
  if(btn)btn.textContent=isOpen?'▼ Expandir detalhes':'▲ Recolher detalhes';
}
function onCardOddInput(matchIdx,marketKey,val,mid){
  var r=AR[matchIdx];var oddVal=parseFloat(val);
  setMatchOdd(r.d,marketKey,isNaN(oddVal)?null:oddVal);
  /* Refresh this card's status panel and header status */
  var bodyEl=document.getElementById('ecard-body-'+matchIdx+'-'+mid);
  var card=document.querySelector('.ecard[data-eid="'+matchIdx+'-'+mid+'"]');
  if(bodyEl){
    /* Re-render decision panel inside the body */
    var panels=bodyEl.querySelectorAll('.entry-status-panel');
    var m=AR[matchIdx].a.methods.find(function(x){return x.id===mid});
    if(m){
      var newPanel=buildEntryStatusPanel(r,m,matchIdx);
      panels.forEach(function(p){p.outerHTML=newPanel});
      /* Update card outer class for status border */
      var st=getEntryFinalStatus(r,m);
      if(card){
        card.className=card.className.replace(/\bs-\w+\b/g,'').trim();
        if(st)card.classList.add('s-'+st.cls);
        var statusRow=card.querySelector('.ecard-status-row');
        if(statusRow){
          var html=st?'<span class="ecard-status ehs-'+st.cls+'">'+st.icon+' '+escHtml(st.label.split('—')[0].trim())+'</span>':'<span class="ecard-status empty">⏳ aguardando odd</span>';
          statusRow.innerHTML=html;
        }
      }
    }
  }
  /* Also keep match-view panel in sync if it's open */
  var matchInp=document.querySelector('input[data-mooddinp="'+matchIdx+'|'+marketKey+'"]');
  if(matchInp)matchInp.value=val;
  updateGlobalCounts();
}

function renderMatchList(){
  var ml=$('#ml');ml.innerHTML='';
  var totConf=0,totHv=0,matchesWithEntries=0,totApproved=0,totWait=0,totReject=0;
  AR.forEach(function(r,idx){
    var conf=r.a.methods.filter(function(m){return m.verdict==='CONFIRMADA'});
    var poss=r.a.methods.filter(function(m){return m.verdict==='POSSÍVEL'});
    var entries=INC_POS?conf.concat(poss):conf;
    if(!entries.length)return;
    entries.sort(function(a,b){var stA=getEntryFinalStatus(r,a),stB=getEntryFinalStatus(r,b);var rA=statusRank(stA),rB=statusRank(stB);if(rA!==rB)return rA-rB;var pA=getProbSuccess(r,a),pB=getProbSuccess(r,b);if(Math.abs(pA-pB)>0.03)return pB-pA;return b.score-a.score});
    matchesWithEntries++;totConf+=conf.length;
    entries.forEach(function(m){if(m.highValue)totHv++;var st=getEntryFinalStatus(r,m);if(st){if(st.cls==='execute'||st.cls==='hv')totApproved++;else if(st.cls==='wait')totWait++;else if(st.cls==='no')totReject++}});
    ml.appendChild(renderMatch(r,idx,entries,conf.length,poss.length));
  });
  $('#totConf').textContent=totConf;$('#totHv').textContent=totHv;$('#totMatches').textContent=matchesWithEntries;
  $('#badgeApproved').style.display=totApproved>0?'inline-flex':'none';
  $('#badgeWait').style.display=totWait>0?'inline-flex':'none';
  $('#badgeReject').style.display=totReject>0?'inline-flex':'none';
  $('#totApproved').textContent=totApproved;$('#totWait').textContent=totWait;$('#totReject').textContent=totReject;
  $('#emptyConf').style.display=matchesWithEntries===0?'block':'none';
}

function getRelevantKeysForMatch(r){
  var entries=r.a.methods.filter(function(m){return m.verdict==='CONFIRMADA'||(INC_POS&&m.verdict==='POSSÍVEL')});
  var keySet={};
  // Always-show markets (from user settings)
  ALWAYS_SHOW_KEYS.forEach(function(k){if(MARKET_LIBRARY[k])keySet[k]=true});
  // Methods' required markets
  entries.forEach(function(m){
    var pk=getMethodPrimaryMarketKey(m,r);
    if(pk && MARKET_LIBRARY[pk]) keySet[pk]=true;
    var ks=METHOD_MARKETS[m.id]||[];
    ks.forEach(function(k){if(MARKET_LIBRARY[k])keySet[k]=true});
  });
  return Object.keys(keySet);
}

function renderMatchOddsPanel(r,idx){
  var d=r.d;
  var allKeys=getRelevantKeysForMatch(r);
  var matchK=matchKey(d);
  var currOdds=MATCH_ODDS[matchK]||{};
  var filledCount=allKeys.filter(function(k){return currOdds[k]}).length;
  var grouped={};
  MARKET_GROUPS.forEach(function(g){grouped[g]=[]});
  allKeys.forEach(function(k){var mkt=MARKET_LIBRARY[k];if(mkt&&grouped[mkt.group]!==undefined){grouped[mkt.group].push(k)}});
  var sectionsHtml='';
  MARKET_GROUPS.forEach(function(g){
    var keys=grouped[g];if(!keys.length)return;
    var cellsHtml=keys.map(function(k){
      var mkt=MARKET_LIBRARY[k];
      var savedOdd=currOdds[k]||'';
      var evCls='',evBadge='';
      if(savedOdd){var calc=calcMarketEV(r,k,parseFloat(savedOdd));if(calc){if(calc.ev>0.03){evCls=' ev-pos';evBadge='<span class="ev-badge pos">+EV</span>'}else if(calc.ev<-0.05){evCls=' ev-neg';evBadge='<span class="ev-badge neg">-EV</span>'}}}
      var hasVal=savedOdd?' has-val':'';
      var implied='';
      if(savedOdd){var ip=(1/parseFloat(savedOdd)*100).toFixed(0);implied='<span class="imp">'+ip+'%</span>'}
      return '<div class="mo-cell'+hasVal+evCls+'" data-mocell="'+idx+'-'+k+'">'+evBadge+'<label><span>'+escHtml(mkt.short)+'</span>'+implied+'</label><input type="number" step="0.01" min="1.01" max="999" placeholder="0.00" value="'+escHtml(savedOdd)+'" data-mooddinp="'+idx+'|'+k+'" oninput="onMatchOddInput('+idx+',\''+k+'\')"></div>';
    }).join('');
    sectionsHtml+='<div class="mo-section"><div class="mo-section-h">'+escHtml(g)+'</div><div class="mo-grid">'+cellsHtml+'</div></div>';
  });
  if(!sectionsHtml){sectionsHtml='<div style="padding:var(--sp-4);text-align:center;color:var(--text-tertiary);font-size:var(--fs-sm);font-style:italic">Nenhum mercado selecionado. Configure em ⚙ Configurações.</div>'}
  return '<div class="match-odds" id="mo-panel-'+idx+'"><div class="mo-head"><div class="mo-head-title"><h3>💰 Odds dos Mercados desta partida</h3></div><div class="mo-head-actions"><button class="btn acc sm" onclick="openPasteModal('+idx+')">📋 Colar odds em massa</button><div class="mo-fill-stats"><span class="stat '+(filledCount===allKeys.length&&allKeys.length>0?'filled':'empty')+'"><span id="mo-fill-'+idx+'">'+filledCount+'</span>/<span id="mo-total-'+idx+'">'+allKeys.length+'</span> odds</span></div></div></div>'+sectionsHtml+'<div class="mo-actions"><button class="btn gld sm" onclick="reanalyzeMatch('+idx+')">🔄 Aplicar e Reordenar entradas</button><button class="btn alt sm" onclick="clearMatchOdds('+idx+')">🗑 Limpar odds desta partida</button></div></div>';
}

function onMatchOddInput(idx,marketKey){
  var inp=document.querySelector('input[data-mooddinp="'+idx+'|'+marketKey+'"]');
  if(!inp)return;
  var oddVal=parseFloat(inp.value);
  var r=AR[idx];
  setMatchOdd(r.d,marketKey,isNaN(oddVal)?null:oddVal);
  var cell=document.querySelector('[data-mocell="'+idx+'-'+marketKey+'"]');
  if(cell){
    cell.classList.remove('has-val','ev-pos','ev-neg');
    var existingBadge=cell.querySelector('.ev-badge');if(existingBadge)existingBadge.remove();
    var labelImp=cell.querySelector('label .imp');if(labelImp)labelImp.remove();
    if(!isNaN(oddVal)&&oddVal>=1.01){
      cell.classList.add('has-val');
      var calc=calcMarketEV(r,marketKey,oddVal);
      if(calc){
        var ip=(1/oddVal*100).toFixed(0);
        var label=cell.querySelector('label span:first-child');
        if(label){var sp=document.createElement('span');sp.className='imp';sp.textContent=ip+'%';label.parentNode.appendChild(sp)}
        if(calc.ev>0.03){cell.classList.add('ev-pos');var b=document.createElement('span');b.className='ev-badge pos';b.textContent='+EV';cell.insertBefore(b,cell.firstChild)}
        else if(calc.ev<-0.05){cell.classList.add('ev-neg');var b=document.createElement('span');b.className='ev-badge neg';b.textContent='-EV';cell.insertBefore(b,cell.firstChild)}
      }
    }
  }
  var totalEl=document.getElementById('mo-total-'+idx);
  var fillEl=document.getElementById('mo-fill-'+idx);
  if(totalEl&&fillEl){
    var allK=getRelevantKeysForMatch(r);
    var matchK=matchKey(r.d);
    var currOdds=MATCH_ODDS[matchK]||{};
    var fillN=allK.filter(function(k){return currOdds[k]}).length;
    fillEl.textContent=fillN;
    var statEl=fillEl.parentElement;
    if(statEl)statEl.className='stat '+(fillN===allK.length&&allK.length>0?'filled':'empty');
  }
  updateEntryStatusForMatch(idx);
  /* Sync flat grid card if visible */
  if(CURRENT_VIEW==='grid'){renderFlatGrid()}
}

function reanalyzeMatch(idx){
  var mb=document.querySelector('.match[data-idx="'+idx+'"]');
  if(!mb)return;
  var wasOpen=mb.classList.contains('open');
  var openEntries={};mb.querySelectorAll('.entry.open').forEach(function(e){openEntries[e.dataset.eid]=true});
  var r=AR[idx];
  var conf=r.a.methods.filter(function(m){return m.verdict==='CONFIRMADA'});
  var poss=r.a.methods.filter(function(m){return m.verdict==='POSSÍVEL'});
  var entries=INC_POS?conf.concat(poss):conf;
  entries.sort(function(a,b){var stA=getEntryFinalStatus(r,a),stB=getEntryFinalStatus(r,b);var rA=statusRank(stA),rB=statusRank(stB);if(rA!==rB)return rA-rB;var pA=getProbSuccess(r,a),pB=getProbSuccess(r,b);if(Math.abs(pA-pB)>0.03)return pB-pA;return b.score-a.score});
  var newMb=renderMatch(r,idx,entries,conf.length,poss.length);
  if(!wasOpen)newMb.classList.remove('open');
  newMb.querySelectorAll('.entry').forEach(function(e){if(openEntries[e.dataset.eid])e.classList.add('open');else e.classList.remove('open')});
  mb.replaceWith(newMb);
  updateGlobalCounts();
  toast('🔄 Entradas reordenadas pela odd!');
}

function clearMatchOdds(idx){
  if(!confirm('Limpar todas as odds desta partida?'))return;
  var r=AR[idx];var k=matchKey(r.d);
  delete MATCH_ODDS[k];localStorage.setItem('match_odds',JSON.stringify(MATCH_ODDS));
  reanalyzeMatch(idx);
}

function updateGlobalCounts(){
  var totApproved=0,totWait=0,totReject=0;
  AR.forEach(function(r){
    var entries=r.a.methods.filter(function(m){return m.verdict==='CONFIRMADA'||(INC_POS&&m.verdict==='POSSÍVEL')});
    entries.forEach(function(m){var st=getEntryFinalStatus(r,m);if(st){if(st.cls==='execute'||st.cls==='hv')totApproved++;else if(st.cls==='wait')totWait++;else if(st.cls==='no')totReject++}});
  });
  $('#badgeApproved').style.display=totApproved>0?'inline-flex':'none';
  $('#badgeWait').style.display=totWait>0?'inline-flex':'none';
  $('#badgeReject').style.display=totReject>0?'inline-flex':'none';
  $('#totApproved').textContent=totApproved;$('#totWait').textContent=totWait;$('#totReject').textContent=totReject;
}

function updateEntryStatusForMatch(idx){
  var r=AR[idx];
  r.a.methods.forEach(function(m){
    var panel=document.getElementById('status-panel-'+idx+'-'+m.id);
    if(panel){panel.outerHTML=buildEntryStatusPanel(r,m,idx)}
    var st=getEntryFinalStatus(r,m);
    var entry=document.querySelector('.entry[data-eid="'+idx+'-'+m.id+'"]');
    if(entry){
      entry.className=entry.className.replace(/\bs-\w+\b/g,'').trim();
      if(st)entry.classList.add('s-'+st.cls);
      var headR=entry.querySelector('.entry-r');
      var existing=entry.querySelector('.entry-head-status');
      if(existing)existing.remove();
      if(st&&headR){var span=document.createElement('span');span.className='entry-head-status ehs-'+st.cls;span.textContent=st.icon+' '+st.label.split('—')[0].trim();headR.insertBefore(span,headR.firstChild)}
    }
  });
  updateGlobalCounts();
}

/* PASTE */
function openPasteModal(idx){PASTE_CTX={idx:idx,mode:'smart',parsed:[]};$('#pasteInput').value='';$('#pastePreview').innerHTML='<div class="preview-empty">Cole as odds acima</div>';$('#pasteCount').textContent='0';document.querySelectorAll('#pasteModal .mode-tab').forEach(function(t){t.classList.toggle('on',t.dataset.mode==='smart')});updatePasteModeDesc();$('#pasteModal').classList.add('on');setTimeout(function(){$('#pasteInput').focus()},100)}
function closePaste(){$('#pasteModal').classList.remove('on');PASTE_CTX={idx:null,mode:'smart',parsed:[]}}
function setPasteMode(mode){PASTE_CTX.mode=mode;document.querySelectorAll('#pasteModal .mode-tab').forEach(function(t){t.classList.toggle('on',t.dataset.mode===mode)});updatePasteModeDesc();updatePastePreview()}
function updatePasteModeDesc(){var desc=$('#pasteDesc');if(PASTE_CTX.mode==='smart'){desc.innerHTML='Detecta nomes + odds: <code>Casa 1.85 X 3.40 Fora 4.20</code> ou <code>Over 2.5: 1.80</code>'}else{desc.innerHTML='Distribui na ordem dos campos: <code>1.85 3.40 4.20 1.80</code>'}}
function detectMarketFromText(text,availableKeys){var norm=text.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'').replace(/\s+/g,' ').trim();var best=null,bestLen=0;availableKeys.forEach(function(k){var mkt=MARKET_LIBRARY[k];if(!mkt||!mkt.aliases)return;mkt.aliases.forEach(function(alias){var aliasNorm=alias.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'');var re;if(aliasNorm.length<=2){re=new RegExp('(?:^|\\s|:)'+aliasNorm.replace(/[.*+?^${}()|[\]\\]/g,'\\$&')+'(?:$|\\s|:|\\.)','i')}else{re=new RegExp(aliasNorm.replace(/[.*+?^${}()|[\]\\]/g,'\\$&'),'i')}if(re.test(norm)&&aliasNorm.length>bestLen){best=k;bestLen=aliasNorm.length}})});return best}
function parsePastedOdds(text,availableKeys){var result=[];if(PASTE_CTX.mode==='sequence'){var matches=text.match(/\d+[,.]\d+|\d+/g)||[];var seq=0;matches.forEach(function(m){var n=parseFloat(m.replace(',','.'));if(!isNaN(n)&&n>=1.01&&n<=999){var key=seq<availableKeys.length?availableKeys[seq]:null;result.push({marketKey:key,odd:n,originalText:m});seq++}})}else{var lines=text.split(/[\n\r]+/);var usedKeys={};lines.forEach(function(line){line=line.trim();if(!line)return;var oddPattern=/(\d+[,.]\d+|\d+\.\d+)/g;var oddMatches=[];var m;while((m=oddPattern.exec(line))!==null){var n=parseFloat(m[1].replace(',','.'));if(!isNaN(n)&&n>=1.01&&n<=999){oddMatches.push({val:n,index:m.index,len:m[0].length,text:m[0]})}}if(!oddMatches.length)return;oddMatches.forEach(function(om,i){var labelStart=i===0?0:(oddMatches[i-1].index+oddMatches[i-1].len);var labelEnd=om.index;var labelText=line.substring(labelStart,labelEnd).replace(/[:|;,]/g,' ').trim();var availableForThis=availableKeys.filter(function(k){return !usedKeys[k]});var key=labelText?detectMarketFromText(labelText,availableForThis):null;if(key){usedKeys[key]=true;result.push({marketKey:key,odd:om.val,originalText:labelText+' '+om.text,label:labelText})}else{result.push({marketKey:null,odd:om.val,originalText:labelText+' '+om.text,label:labelText})}})});var unmatched=result.filter(function(r){return !r.marketKey});var remainingKeys=availableKeys.filter(function(k){return !usedKeys[k]});if(unmatched.length>0&&remainingKeys.length>0&&unmatched.length<=remainingKeys.length){var rkIdx=0;result.forEach(function(r){if(!r.marketKey&&rkIdx<remainingKeys.length){r.marketKey=remainingKeys[rkIdx];r.autoFilled=true;rkIdx++}})}}return result}
function updatePastePreview(){var input=$('#pasteInput').value;var preview=$('#pastePreview');if(!input.trim()){preview.innerHTML='<div class="preview-empty">Cole as odds acima</div>';$('#pasteCount').textContent='0';PASTE_CTX.parsed=[];return}if(PASTE_CTX.idx==null){preview.innerHTML='';return}var r=AR[PASTE_CTX.idx];var availableKeys=getRelevantKeysForMatch(r);var parsed=parsePastedOdds(input,availableKeys);PASTE_CTX.parsed=parsed;var matchedCount=parsed.filter(function(p){return p.marketKey}).length;$('#pasteCount').textContent=parsed.length+' • '+matchedCount+' mapeadas';if(!parsed.length){preview.innerHTML='<div class="preview-empty">Nenhuma odd encontrada</div>';return}var rowsHtml=parsed.map(function(p){if(p.marketKey){var mkt=MARKET_LIBRARY[p.marketKey];var auto=p.autoFilled?' <small>(auto)</small>':'';return '<div class="row matched"><span>✓ '+escHtml(mkt.short)+auto+'</span><b>'+p.odd.toFixed(2)+'</b></div>'}else{return '<div class="row skipped"><span>? '+escHtml(p.label||'(s/rótulo)')+'</span><span>'+p.odd.toFixed(2)+'</span></div>'}}).join('');preview.innerHTML=rowsHtml}
function applyPaste(){if(PASTE_CTX.idx==null){closePaste();return}var parsed=PASTE_CTX.parsed;if(!parsed.length){toast('Nenhuma odd',true);return}var idx=PASTE_CTX.idx;var r=AR[idx];var appliedCount=0;parsed.forEach(function(p){if(!p.marketKey)return;setMatchOdd(r.d,p.marketKey,p.odd);appliedCount++;var inp=document.querySelector('input[data-mooddinp="'+idx+'|'+p.marketKey+'"]');if(inp){inp.value=p.odd.toFixed(2);var cell=document.querySelector('[data-mocell="'+idx+'-'+p.marketKey+'"]');if(cell){cell.classList.add('flash');setTimeout(function(){cell.classList.remove('flash')},700)}onMatchOddInput(idx,p.marketKey)}});closePaste();if(appliedCount>0){toast('✓ '+appliedCount+' odd(s) aplicadas!');if(CURRENT_VIEW==='grid')renderFlatGrid()}else{toast('Nenhuma odd mapeada',true)}}

function renderMatch(r,idx,entries,confCount,possCount){
  var d=r.d,a=r.a;
  var mb=document.createElement('div');mb.className='match';mb.dataset.idx=idx;
  var status=timeStatus(d.date,d.time);
  var statusCls=status==='live'?' live':status==='soon'?' soon':'';
  var statusIcon=status==='live'?'🔴':status==='soon'?'⏳':'🕐';
  var statusLabel=status==='live'?'AO VIVO':status==='soon'?'EM BREVE':'';
  var dateChip='<span class="chip date" onclick="event.stopPropagation();editField('+idx+',\'date\')"><span class="ic">📅</span><span>'+escHtml(d.date||'Sem data')+'</span><span class="edit-ic">✎</span></span>';
  var timeChip='<span class="chip time'+statusCls+'" onclick="event.stopPropagation();editField('+idx+',\'time\')"><span class="ic">'+statusIcon+'</span><span>'+escHtml(d.time||'--:--')+(statusLabel?' • '+statusLabel:'')+'</span><span class="edit-ic">✎</span></span>';
  var compChip=d.comp?'<span class="chip comp"><span class="ic">🏆</span><span>'+escHtml(d.comp)+'</span></span>':'';
  var roundChip=d.round?'<span class="chip round">'+escHtml(d.round)+'</span>':'';
  var head='<div class="m-head" onclick="toggleMatch('+idx+')"><div class="m-title"><div class="m-teams">'+escHtml(d.home)+(d.homeFmt?' <span class="fmt-badge">'+d.homeFmt+'</span>':'')+' <span class="m-vs">×</span> '+escHtml(d.away)+(d.awayFmt?' <span class="fmt-badge">'+d.awayFmt+'</span>':'')+'</div><div class="m-chips">'+dateChip+timeChip+compChip+roundChip+'</div></div><div class="m-count"><span class="pill suc"><span class="n">'+confCount+'</span> CONFIRMADAS</span>'+(possCount&&INC_POS?'<span class="pill warn"><span class="n">'+possCount+'</span> POSS</span>':'')+'<span class="chev">▼</span></div></div>';
  var poisson='<div class="pois-banner"><div class="pois-banner-l"><div class="pois-stat h"><div class="lbl">Vit. '+escHtml(d.home).substring(0,12)+'</div><div class="v">'+fmt(a.poisson.pHome*100,1)+'%</div></div><div class="pois-stat d"><div class="lbl">Empate</div><div class="v">'+fmt(a.poisson.pDraw*100,1)+'%</div></div><div class="pois-stat a"><div class="lbl">Vit. '+escHtml(d.away).substring(0,12)+'</div><div class="v">'+fmt(a.poisson.pAway*100,1)+'%</div></div></div><div class="pois-banner-r">'+a.poisson.topScores.slice(0,3).map(function(s){return '<div class="ts">'+s.score+'<b>'+fmt(s.p*100,1)+'%</b></div>'}).join('')+'</div></div>';
  var matchOddsHtml=renderMatchOddsPanel(r,idx);
  var entriesHtml='<div class="entries">';
  entries.forEach(function(m,entryIdx){entriesHtml+=renderEntry(r,m,idx,entryIdx)});
  entriesHtml+='</div>';
  mb.innerHTML=head+poisson+matchOddsHtml+entriesHtml;
  return mb;
}

function editField(idx,field){
  var r=AR[idx];if(!r)return;var d=r.d;
  var chips=document.querySelectorAll('.match[data-idx="'+idx+'"] .m-chips .chip');
  var targetChip=null;
  chips.forEach(function(c){if(field==='date'&&c.classList.contains('date'))targetChip=c;if(field==='time'&&c.classList.contains('time'))targetChip=c});
  if(!targetChip)return;
  var currentVal=field==='date'?(d.date||''):(d.time||'');
  var inputType=field==='date'?'text':'time';
  var edit=document.createElement('span');edit.className='chip-edit';
  edit.innerHTML='<span class="ic">'+(field==='date'?'📅':'🕐')+'</span><input type="'+inputType+'" value="'+escHtml(currentVal)+'" placeholder="'+(field==='date'?'DD/MM/AAAA':'HH:MM')+'" id="editInp-'+idx+'-'+field+'"><button onclick="event.stopPropagation();saveField('+idx+',\''+field+'\')">✓</button><button class="cancel" onclick="event.stopPropagation();cancelEdit('+idx+')">✕</button>';
  edit.onclick=function(e){e.stopPropagation()};
  targetChip.replaceWith(edit);
  setTimeout(function(){var inp=document.getElementById('editInp-'+idx+'-'+field);if(inp){inp.focus();if(field==='date')inp.select();inp.addEventListener('keydown',function(e){if(e.key==='Enter'){e.preventDefault();saveField(idx,field)}if(e.key==='Escape'){e.preventDefault();cancelEdit(idx)}})}},10);
}
function saveField(idx,field){var r=AR[idx];if(!r)return;var inp=document.getElementById('editInp-'+idx+'-'+field);if(!inp)return;var val=inp.value.trim();if(field==='date')val=normalizeDate(val);else if(field==='time')val=normalizeTime(val);r.d[field]=val;var k=matchKey(r.d);if(!DT_OVERRIDES[k])DT_OVERRIDES[k]={};DT_OVERRIDES[k][field]=val;localStorage.setItem('dt_overrides',JSON.stringify(DT_OVERRIDES));renderMatchList();toast('✓ atualizado')}
function cancelEdit(idx){renderMatchList()}
function toggleMatch(idx){var mb=document.querySelector('.match[data-idx="'+idx+'"]');if(mb)mb.classList.toggle('open')}
function toggleAll(){var matches=$$('.match');var anyClosed=Array.prototype.some.call(matches,function(m){return !m.classList.contains('open')});matches.forEach(function(m){if(anyClosed)m.classList.add('open');else m.classList.remove('open')})}

function renderEntry(r,m,matchIdx,entryIdx){
  var L=METHOD_LEGENDS[m.id];var d=r.d;
  var hvCls=m.highValue?' hv':'';var openCls=' open';var typeCls=m.type;
  var ps=getProbSuccess(r,m);var psPct=Math.round(ps*100);
  var psClass=psPct>=70?'high':psPct>=50?'mid':'low';
  var finalSt=getEntryFinalStatus(r,m);
  var statusCardCls=finalSt?' s-'+finalSt.cls:'';
  var rankBadge=entryIdx<=2?'<div class="rank r'+(entryIdx+1)+'">'+(entryIdx+1)+'</div>':'<div class="rank">'+(entryIdx+1)+'</div>';
  var headStatus='';
  if(finalSt)headStatus='<span class="entry-head-status ehs-'+finalSt.cls+'">'+finalSt.icon+' '+finalSt.label.split('—')[0].trim()+'</span>';
  var motivo=buildMotivoInformal(r,m,L,ps);
  var critHtml=m.criteria.map(function(c){var ic=c.ok?'✓':'✗';var cls=c.ok?'ok':'no';var w=c.w>1?'<span class="w">peso '+c.w+'</span>':'';var raw=c.raw?' <span style="color:var(--text-tertiary);font-family:JetBrains Mono;font-size:11px">('+escHtml(c.raw)+')</span>':'';return '<li class="'+cls+'"><span class="ic">'+ic+'</span><span class="txt">'+escHtml(c.label)+raw+'</span>'+w+'</li>'}).join('');
  var oddMaxTxt=m.oddMax>=99?'∞':m.oddMax.toFixed(2);
  var statusPanel=buildEntryStatusPanel(r,m,matchIdx);
  return '<div class="entry'+hvCls+openCls+statusCardCls+'" data-eid="'+matchIdx+'-'+m.id+'">'+rankBadge+'<div class="entry-head" onclick="toggleEntry(\''+matchIdx+'-'+m.id+'\')"><div class="entry-l"><div class="entry-num">'+L.label+'</div><div class="entry-text"><div class="entry-name">'+escHtml(L.title)+(m.highValue?' <span class="star">★</span>':'')+'</div><div class="entry-mkt"><span class="tag '+typeCls+'">'+m.type.toUpperCase()+'</span><span class="prob '+psClass+'">'+psPct+'% chance</span><span>📍 '+escHtml(m.market)+'</span></div></div></div><div class="entry-r">'+headStatus+'<div class="entry-score"><span class="n">'+m.score+'</span><span class="pct">/100</span></div><span class="entry-chev">▼</span></div></div><div class="entry-body"><div class="section"><div class="section-h">💡 Por que essa entrada?</div><div class="motivo">'+motivo+'</div></div><div class="section"><div class="section-h gold">🎯 Decisão final</div>'+statusPanel+'</div><div class="section"><div class="section-h">📋 Requisitos</div><div class="req-grid"><div class="req-box entrada"><h5>🟢 Entrada</h5><ul><li><span class="k">Mercado</span><span class="v">'+escHtml(m.market)+'</span></li><li><span class="k">Tipo</span><span class="v">'+m.type.toUpperCase()+'</span></li><li><span class="k">Momento</span><span class="v">'+escHtml(m.entry)+'</span></li>'+(d.date||d.time?'<li><span class="k">Horário</span><span class="v">'+escHtml((d.date||'')+(d.time?' '+d.time:''))+'</span></li>':'')+'<li><span class="k">Stake</span><span class="v">'+m.stake+'</span></li><li><span class="k">Risco</span><span class="v">'+m.risk+'</span></li><li><span class="k">P. ganhar</span><span class="v" style="color:var(--lime)">'+psPct+'%</span></li></ul><div class="odds-box"><div class="ob"><div class="ol">Mín</div><div class="ov">'+m.oddMin.toFixed(2)+'</div></div><div class="ob ideal"><div class="ol">Ideal</div><div class="ov">'+m.oddIdeal.toFixed(2)+'</div></div><div class="ob"><div class="ol">Máx</div><div class="ov">'+oddMaxTxt+'</div></div></div></div><div class="req-box saida"><h5>🟡 Saída</h5><ul><li><span class="k">Gatilho</span><span class="v" style="font-size:12px;text-align:right;line-height:1.4">'+escHtml(m.exit)+'</span></li><li><span class="k">Estilo</span><span class="v">'+(m.type==='back'?'Back':'Lay')+'</span></li><li><span class="k">Veredicto</span><span class="v" style="color:var(--success)">'+m.verdict+'</span></li></ul></div></div></div><div class="section"><div class="section-h suc">✓ Critérios ('+m.criteria.filter(function(c){return c.ok}).length+'/'+m.criteria.length+')</div><ul class="crit">'+critHtml+'</ul></div></div></div>';
}
function toggleEntry(eid){var el=document.querySelector('.entry[data-eid="'+eid+'"]');if(el)el.classList.toggle('open')}

function buildEntryStatusPanel(r,m,idx){
  var primaryKey=getMethodPrimaryMarketKey(m,r);
  if(!primaryKey)return '<div class="entry-status-panel" id="status-panel-'+idx+'-'+m.id+'"><div class="entry-status-empty">📝 Esta entrada é apenas análise estatística (sem odd direta no painel).</div></div>';
  var primaryOdd=getMatchOdd(r.d,primaryKey);
  var allKeys=METHOD_MARKETS[m.id]||[];
  var allFilledOdds={};
  allKeys.forEach(function(k){if((m.id===1||m.id===10||m.id===13)&&k!==primaryKey)return;if(m.id===9&&k!==primaryKey)return;var od=getMatchOdd(r.d,k);if(od)allFilledOdds[k]=od});
  if(!primaryOdd){
    var mkt=MARKET_LIBRARY[primaryKey];
    var label=mkt?mkt.label:'mercado';
    return '<div class="entry-status-panel" id="status-panel-'+idx+'-'+m.id+'"><div class="entry-status-empty">📝 Digite a odd de <b>'+escHtml(label)+'</b> no painel acima para validar</div></div>';
  }
  var st=analyzeOdd(m,parseFloat(primaryOdd),r);
  if(!st)return '<div class="entry-status-panel" id="status-panel-'+idx+'-'+m.id+'"></div>';
  var oddsListHtml=Object.keys(allFilledOdds).map(function(k){var mkt=MARKET_LIBRARY[k];var od=allFilledOdds[k];var calc=calcMarketEV(r,k,parseFloat(od));var evTxt=calc?((calc.ev>=0?'+':'')+(calc.ev*100).toFixed(1)+'%'):'';var evCls=calc?(calc.ev>0.03?'pos':calc.ev<-0.05?'neg':''):'';return '<div class="b"><div class="k">'+escHtml(mkt.short)+'</div><div class="v">'+parseFloat(od).toFixed(2)+'</div><div class="v" style="font-size:10px;color:var(--text-tertiary);font-weight:600">EV <span class="'+evCls+'">'+evTxt+'</span></div></div>'}).join('');
  var stake100=100;var lucroEx,perdaEx;if(m.type==='back'){lucroEx=stake100*(parseFloat(primaryOdd)-1);perdaEx=stake100}else{lucroEx=stake100;perdaEx=stake100*(parseFloat(primaryOdd)-1)}
  return '<div class="entry-status-panel os-'+st.cls+'" id="status-panel-'+idx+'-'+m.id+'"><div class="entry-status-panel-h">📊 Análise da odd '+(MARKET_LIBRARY[primaryKey]?MARKET_LIBRARY[primaryKey].short:'')+': <b>'+parseFloat(primaryOdd).toFixed(2)+'</b></div><div class="entry-status-decision"><span class="ic">'+st.icon+'</span><div class="txt"><span class="lbl">'+escHtml(st.label)+'</span><span class="msg">'+escHtml(st.msg)+'</span></div></div><div class="entry-status-grid"><div class="b"><div class="k">Stake</div><div class="v">R$ 100,00</div></div><div class="b"><div class="k">Lucro</div><div class="v pos">+R$ '+lucroEx.toFixed(2)+'</div></div><div class="b"><div class="k">Perda</div><div class="v neg">-R$ '+perdaEx.toFixed(2)+'</div></div><div class="b"><div class="k">Chance</div><div class="v" style="color:var(--lime)">'+(st.ps*100).toFixed(0)+'%</div></div><div class="b"><div class="k">EV</div><div class="v '+(st.ev>=0?'pos':'neg')+'">'+(st.ev>=0?'+':'')+(st.ev*100).toFixed(1)+'%</div></div>'+oddsListHtml+'</div></div>';
}

function buildMotivoInformal(r,m,L,ps){
  var d=r.d,a=r.a;var psPct=(ps*100).toFixed(0);
  var passed=m.criteria.filter(function(c){return c.ok});var failed=m.criteria.filter(function(c){return !c.ok});
  var txt='';
  if(m.id===1||m.id===10||m.id===13){var ppgD=fmt(Math.abs(a.ppgDiff),2);var emCasa=a.fav==='home'?'em casa':'fora';txt+='<p>Olha só esse cenário 👀: o <b class="hi">'+escHtml(a.favName)+'</b> tá numa fase bem superior ao <b>'+escHtml(a.udName)+'</b>. Tem <b class="hi">'+ppgD+'</b> pontos a mais por jogo, joga '+emCasa+' e modelo aponta <b class="hi">'+psPct+'%</b> de chance.</p>'}
  else if(m.id===2||m.id===11){txt+='<p>Cenário de <b class="hi">gols à vontade</b> 🎯! BTTS em <b class="hi">'+fmt(a.bttsAvg,0)+'%</b>, Over 2.5 em <b class="hi">'+fmt(a.o25Avg,0)+'%</b>. Modelo dá <b class="hi">'+psPct+'%</b> de chance.</p>'}
  else if(m.id===3){var favG=fmt(a.fav==='home'?d.H.avgScored:d.A.avgScored,2);var advS=fmt(a.fav==='home'?d.A.avgConceded:d.H.avgConceded,2);txt+='<p><b>Goleada à vista</b> 💥! Favorito marca <b class="hi">'+favG+'</b> gols/jogo, adversário sofre <b class="lo">'+advS+'</b>. <b class="hi">'+psPct+'%</b> de chance de 4+ gols.</p>'}
  else if(m.id===4){txt+='<p>Estratégia <b class="mid">LIVE</b> ⚡ — entrada nos minutos 68-72. Chance de gol após 70\': <b class="hi">'+psPct+'%</b>.</p>'}
  else if(m.id===5||m.id===6){var cs={5:'1-0',6:'0-1'}[m.id];var pCS={5:a.poisson.matrix[1][0],6:a.poisson.matrix[0][1]}[m.id]*100;txt+='<p>Placar <b class="lo">'+cs+'</b> é pouco provável: só <b class="lo">'+fmt(pCS,1)+'%</b>. Lay tem <b class="hi">'+psPct+'%</b> de chance.</p>'}
  else if(m.id===9){txt+='<p>Diferença gritante 🦓: <b class="hi">'+escHtml(a.favName)+'</b> é favorito. Lay no azarão tem <b class="hi">'+psPct+'%</b>.</p>'}
  else if(m.id===12){txt+='<p>Jogo com <b>gols dos dois lados</b> ⚽. Empate cai pra <b class="lo">'+fmt(a.poisson.pDraw*100,1)+'%</b>. Lay Empate: <b class="hi">'+psPct+'%</b>.</p>'}
  
  txt+='<p>📊 <b>Critérios:</b> '+passed.length+'/'+m.criteria.length+' (score <b class="hi">'+m.score+'%</b>). ';
  if(passed.length>=3){var top2=passed.sort(function(a,b){return (b.w||1)-(a.w||1)}).slice(0,2).map(function(c){return c.label}).join(' e ');txt+='Pontos fortes: <b>'+top2+'</b>. '}
  if(failed.length>0&&failed.length<=2){var topF=failed.sort(function(a,b){return (b.w||1)-(a.w||1)}).slice(0,1).map(function(c){return c.label}).join(', ');txt+='<span class="lo">⚠ '+topF+'</span>.'}
  txt+='</p>';
  if(m.highValue){txt+='<p>⭐ <b class="gold">High Value:</b> quanto maior a odd, maior o EV.</p>'}
  if(m.type==='lay'){txt+='<p style="font-size:12px;color:var(--text-tertiary);border-top:1px dashed var(--border);padding-top:8px;margin-top:12px"><b style="color:var(--warning)">⚠ Regra do Lay:</b> odd >20 = aguardar live.</p>'}
  return txt;
}

function exportCSV(){
  var rows=[['Partida','Data','Horario','Metodo','Tipo','Mercado','Score','Prob','Veredicto','Sua_Odd','Status','Decisao']];
  AR.forEach(function(r){var d=r.d;r.a.methods.forEach(function(m){var keep=m.verdict==='CONFIRMADA'||(INC_POS&&m.verdict==='POSSÍVEL');if(!keep)return;var L=METHOD_LEGENDS[m.id];var ps=getProbSuccess(r,m);var pk=getMethodPrimaryMarketKey(m,r);var savedOdd=pk?getMatchOdd(d,pk):null;var statusOdd='',decisao='Aguardando odd';if(savedOdd){var ar=analyzeOdd(m,parseFloat(savedOdd),r);if(ar){statusOdd=ar.label;decisao=ar.cls==='execute'?'EXECUTAR':ar.cls==='hv'?'ALTO VALOR':ar.cls==='wait'?'AGUARDAR LIVE':ar.cls==='warn'?'CUIDADO':'REJEITAR'}}rows.push([d.home+' x '+d.away,d.date||'',d.time||'',L.label,m.type.toUpperCase(),m.market,m.score+'%',(ps*100).toFixed(0)+'%',m.verdict,savedOdd?parseFloat(savedOdd).toFixed(2):'',statusOdd,decisao])})});
  if(rows.length===1){toast('Nenhuma entrada',true);return}
  var csv='\uFEFF'+rows.map(function(r){return r.map(function(c){return '"'+String(c).replace(/"/g,'""')+'"'}).join(',')}).join('\n');
  var blob=new Blob([csv],{type:'text/csv;charset=utf-8'});var url=URL.createObjectURL(blob);
  var a=document.createElement('a');a.href=url;a.download='Entradas_'+new Date().toISOString().slice(0,10)+'.csv';
  document.body.appendChild(a);a.click();document.body.removeChild(a);URL.revokeObjectURL(url);
  toast('📊 CSV exportado!');
}

async function exportPDF(){
  if(!AR.length){toast('Processe partidas primeiro',true);return}
  try{
    var {jsPDF}=window.jspdf;
    var doc=new jsPDF({unit:'mm',format:'a4'});
    var pageW=doc.internal.pageSize.getWidth(),pageH=doc.internal.pageSize.getHeight();
    var margin=14,y=margin;var todayStr=new Date().toLocaleString('pt-BR');
    function drawLogo(x,yy,size){doc.setFillColor(59,130,246);doc.circle(x+size/2,yy+size/2,size/2,'F');doc.setTextColor(255,255,255);doc.setFont('helvetica','bold');doc.setFontSize(size*.5);doc.text('AO',x+size/2,yy+size/2+size*.15,{align:'center'})}
    function header(){doc.setFillColor(15,21,36);doc.rect(0,0,pageW,4,'F');if(LOGO_B64){try{doc.addImage(LOGO_B64,'PNG',margin,8,14,14)}catch(e){drawLogo(margin,8,14)}}else{drawLogo(margin,8,14)}doc.setTextColor(15,21,36);doc.setFont('helvetica','bold');doc.setFontSize(13);doc.text('Analisador Operacional',margin+20,14);doc.setFont('helvetica','normal');doc.setFontSize(8);doc.setTextColor(100,116,139);doc.text('Análise + Odds por partida',margin+20,19);doc.text(todayStr,pageW-margin,13,{align:'right'});doc.text('v7.3.1',pageW-margin,18,{align:'right'});doc.setDrawColor(226,232,240);doc.setLineWidth(0.3);doc.line(margin,25,pageW-margin,25)}
    function footer(pg,tot){doc.setFontSize(7);doc.setTextColor(148,163,184);doc.text('Página '+pg+' de '+tot,pageW-margin,pageH-6,{align:'right'});doc.text('Apostas envolvem risco.',margin,pageH-6)}
    function checkPage(needed){if(y+needed>pageH-15){doc.addPage();y=margin;header();y=30}}
    doc.setFillColor(15,21,36);doc.rect(0,0,pageW,pageH,'F');doc.setFillColor(59,130,246);doc.rect(0,0,pageW,pageH*0.5,'F');
    if(LOGO_B64){try{doc.addImage(LOGO_B64,'PNG',pageW/2-15,50,30,30)}catch(e){drawLogo(pageW/2-15,50,30)}}else{drawLogo(pageW/2-15,50,30)}
    doc.setTextColor(255,255,255);doc.setFont('helvetica','bold');doc.setFontSize(28);doc.text('Analisador Operacional',pageW/2,100,{align:'center'});
    doc.setFont('helvetica','normal');doc.setFontSize(13);doc.text('Entradas + Odds dos Mercados',pageW/2,110,{align:'center'});
    doc.setTextColor(180,200,220);doc.setFontSize(9);doc.text('Gerado em '+todayStr,pageW/2,pageH-30,{align:'center'});
    AR.forEach(function(r,idx){
      var conf=r.a.methods.filter(function(m){return m.verdict==='CONFIRMADA'});
      var poss=r.a.methods.filter(function(m){return m.verdict==='POSSÍVEL'});
      var entries=INC_POS?conf.concat(poss):conf;if(!entries.length)return;
      entries.sort(function(a,b){var stA=getEntryFinalStatus(r,a),stB=getEntryFinalStatus(r,b);var rA=statusRank(stA),rB=statusRank(stB);if(rA!==rB)return rA-rB;var pA=getProbSuccess(r,a),pB=getProbSuccess(r,b);if(Math.abs(pA-pB)>0.03)return pB-pA;return b.score-a.score});
      doc.addPage();doc.setFillColor(255,255,255);doc.rect(0,0,pageW,pageH,'F');y=margin;header();y=30;
      var d=r.d,a=r.a;
      doc.setFillColor(59,130,246);doc.roundedRect(margin,y,pageW-2*margin,28,3,3,'F');
      doc.setTextColor(255,255,255);doc.setFont('helvetica','bold');doc.setFontSize(15);doc.text(d.home+' x '+d.away,margin+4,y+10);
      doc.setFont('helvetica','bold');doc.setFontSize(10);var dtTxt=(d.date||'Sem data')+(d.time?' • '+d.time+'h':'');doc.text('📅 '+dtTxt,margin+4,y+18);
      var info='';if(d.comp)info+=d.comp;if(d.round)info+=(info?' • ':'')+d.round;
      if(info){doc.setFont('helvetica','normal');doc.setFontSize(8);doc.text(info,pageW-margin-3,y+18,{align:'right'})}
      y+=34;
      var matchK=matchKey(d);var oddsObj=MATCH_ODDS[matchK]||{};var oddsKeys=Object.keys(oddsObj).filter(function(k){return MARKET_LIBRARY[k]});
      if(oddsKeys.length){
        checkPage(12+oddsKeys.length*3.5);
        doc.setFillColor(254,243,199);doc.roundedRect(margin,y,pageW-2*margin,8+oddsKeys.length*3.5,2,2,'F');
        doc.setFillColor(245,158,11);doc.rect(margin,y,2,8+oddsKeys.length*3.5,'F');
        doc.setTextColor(180,120,0);doc.setFont('helvetica','bold');doc.setFontSize(8);doc.text('ODDS DA PARTIDA:',margin+6,y+5);
        doc.setFont('helvetica','normal');doc.setFontSize(8);doc.setTextColor(30,41,59);
        var rowY=y+9;
        oddsKeys.forEach(function(k){var mkt=MARKET_LIBRARY[k];if(!mkt)return;var od=oddsObj[k];var calc=calcMarketEV(r,k,parseFloat(od));var evTxt=calc?(' • EV '+(calc.ev>=0?'+':'')+(calc.ev*100).toFixed(1)+'%'):'';doc.text('• '+mkt.label+': '+parseFloat(od).toFixed(2)+evTxt,margin+6,rowY);rowY+=3.5});
        y=rowY+3;
      }
      entries.forEach(function(m,mi){
        var L=METHOD_LEGENDS[m.id];var passed=m.criteria.filter(function(c){return c.ok});
        var cardH=14+22+(m.criteria.length*3.5)+5;checkPage(cardH);
        var st=getEntryFinalStatus(r,m);
        var bdColor=[16,185,129];if(st){if(st.cls==='hv')bdColor=[251,191,36];else if(st.cls==='wait')bdColor=[245,158,11];else if(st.cls==='no')bdColor=[239,68,68];else if(st.cls==='warn')bdColor=[252,211,77]}
        doc.setFillColor(255,255,255);doc.setDrawColor(bdColor[0],bdColor[1],bdColor[2]);doc.setLineWidth(0.8);
        doc.roundedRect(margin,y,pageW-2*margin,cardH,2,2,'FD');
        doc.setFillColor(bdColor[0],bdColor[1],bdColor[2]);doc.roundedRect(margin,y,pageW-2*margin,11,2,2,'F');doc.rect(margin,y+5,pageW-2*margin,6,'F');
        doc.setTextColor(255,255,255);doc.setFont('helvetica','bold');doc.setFontSize(11);
        doc.text('#'+(mi+1)+' '+L.label+' — '+L.title+(m.highValue?' ★':''),margin+4,y+7);
        var ps=getProbSuccess(r,m);doc.setFontSize(11);
        var rightTxt=st?(st.label.split('—')[0].trim()):Math.round(ps*100)+'%';
        doc.text(rightTxt,pageW-margin-4,y+7,{align:'right'});y+=14;
        doc.setTextColor(100,116,139);doc.setFont('helvetica','bold');doc.setFontSize(7);doc.text('POR QUE ENTRAR:',margin+3,y);
        doc.setFont('helvetica','normal');doc.setFontSize(8);doc.setTextColor(40,50,70);
        var motivoTxt='Critérios: '+passed.length+'/'+m.criteria.length+' (score '+m.score+'%). Prob: '+Math.round(ps*100)+'%. ';
        var pk=getMethodPrimaryMarketKey(m,r);var savedOdd=pk?getMatchOdd(d,pk):null;
        if(savedOdd){motivoTxt+='Sua odd: '+parseFloat(savedOdd).toFixed(2)+'. '}
        motivoTxt+='Cenário ideal: '+L.ideal;
        var split=doc.splitTextToSize(motivoTxt,pageW-2*margin-6);doc.text(split,margin+3,y+4);y+=4+split.length*3.5+3;
        if(st){doc.setFontSize(8);doc.setTextColor(bdColor[0],bdColor[1],bdColor[2]);doc.setFont('helvetica','bold');doc.text('DECISÃO: '+st.label,margin+3,y);y+=5;doc.setFont('helvetica','normal');doc.setFontSize(7);doc.setTextColor(80,80,80);var msgSpl=doc.splitTextToSize(st.msg,pageW-2*margin-6);doc.text(msgSpl.slice(0,2),margin+3,y);y+=msgSpl.slice(0,2).length*3.5+2}
        doc.setTextColor(100,116,139);doc.setFont('helvetica','bold');doc.setFontSize(7);doc.text('CRITÉRIOS:',margin+3,y);y+=3.5;
        doc.setFont('helvetica','normal');doc.setFontSize(7);
        m.criteria.forEach(function(c){doc.setTextColor(c.ok?16:239,c.ok?185:68,c.ok?129:68);doc.text(c.ok?'✓':'✗',margin+3,y);doc.setTextColor(30,41,59);var txt=c.label+(c.raw?' ('+c.raw+')':'');if(c.w>1)txt+=' [w'+c.w+']';doc.text(txt,margin+8,y);y+=3.5});
        y+=4;
      });
    });
    var pageCount=doc.getNumberOfPages();for(var p=2;p<=pageCount;p++){doc.setPage(p);footer(p,pageCount)}
    doc.save('Entradas_'+new Date().toISOString().slice(0,10)+'.pdf');
    toast('📄 PDF exportado!');
  }catch(err){console.error(err);toast('Erro: '+err.message,true)}
}

window.addEventListener('DOMContentLoaded',function(){applyLogo()});
</script>

</body>
</html>
