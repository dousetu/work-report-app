import React, { useEffect, useMemo, useState, memo } from "react";

const pad = (n) => String(n).padStart(2, "0");
const weekNames = ["日", "月", "火", "水", "木", "金", "土"];

const holidays2026 = {
  "2026-01-01": "元日",
  "2026-01-12": "成人の日",
  "2026-02-11": "建国記念の日",
  "2026-02-23": "天皇誕生日",
  "2026-03-20": "春分の日",
  "2026-04-29": "昭和の日",
  "2026-05-03": "憲法記念日",
  "2026-05-04": "みどりの日",
  "2026-05-05": "こどもの日",
  "2026-05-06": "休日",
  "2026-07-20": "海の日",
  "2026-08-11": "山の日",
  "2026-09-21": "敬老の日",
  "2026-09-22": "休日",
  "2026-09-23": "秋分の日",
  "2026-10-12": "スポーツの日",
  "2026-11-03": "文化の日",
  "2026-11-23": "勤労感謝の日",
};

const workTypes = ["通常", "有休", "有給", "公休", "欠勤"];
const timeOptions = Array.from({ length: 48 }, (_, index) => {
  const hour = Math.floor(index / 2);
  const minute = index % 2 === 0 ? "00" : "30";
  return `${pad(hour)}:${minute}`;
});
const breakOptions = ["0", "0.5", "1", "1.5", "2", "2.5", "3", "3.5", "4"];
const inputClass = "h-9 w-full rounded-lg border border-slate-300 bg-white px-2 text-sm outline-none focus:border-slate-500";
const BOM = "\uFEFF";
const NEW_LINE = "\n";
const STORAGE_KEY = "work-report-app-v2";

function safeLocalStorageGet(key) {
  try {
    if (typeof window === "undefined" || !window.localStorage) return null;
    return window.localStorage.getItem(key);
  } catch (error) {
    return null;
  }
}

function safeLocalStorageSet(key, value) {
  try {
    if (typeof window === "undefined" || !window.localStorage) return;
    window.localStorage.setItem(key, value);
  } catch (error) {
    // 保存できない環境では何もしない
  }
}

function safeLocalStorageRemove(key) {
  try {
    if (typeof window === "undefined" || !window.localStorage) return;
    window.localStorage.removeItem(key);
  } catch (error) {
    // 削除できない環境では何もしない
  }
}

function toNumber(value) {
  const n = Number(value);
  return Number.isFinite(n) ? n : 0;
}

function toMin(time) {
  if (!time || typeof time !== "string" || !time.includes(":")) return null;
  const [h, m] = time.split(":").map(Number);
  if (!Number.isFinite(h) || !Number.isFinite(m)) return null;
  return h * 60 + m;
}

function hoursBetween(start, end) {
  const s = toMin(start);
  let e = toMin(end);
  if (s === null || e === null) return 0;
  if (e < s) e += 24 * 60;
  return (e - s) / 60;
}

function nightHours(start, end, nightBreak) {
  const s0 = toMin(start);
  let e0 = toMin(end);
  if (s0 === null || e0 === null) return 0;
  if (e0 < s0) e0 += 24 * 60;

  const ranges = [
    [0, 5 * 60],
    [22 * 60, 29 * 60],
  ];

  let totalMinutes = 0;
  for (const [rangeStart, rangeEnd] of ranges) {
    const overlapStart = Math.max(s0, rangeStart);
    const overlapEnd = Math.min(e0, rangeEnd);
    if (overlapEnd > overlapStart) totalMinutes += overlapEnd - overlapStart;
  }

  return Math.max(0, totalMinutes / 60 - toNumber(nightBreak));
}

function calcRow(row) {
  if (row.workType === "有休" || row.workType === "有給") return { actual: 7, overtime: 0, night: 0 };
  if (row.workType === "公休" || row.workType === "欠勤") return { actual: 0, overtime: 0, night: 0 };
  if (!row.start || !row.end) return { actual: 0, overtime: 0, night: 0 };

  const normalBreak = toNumber(row.breakBefore22);
  const nightBreak = toNumber(row.breakAfter22);
  const actual = Math.max(0, hoursBetween(row.start, row.end) - normalBreak - nightBreak);
  const overtime = Math.max(0, actual - 8);
  const night = nightHours(row.start, row.end, nightBreak);

  return { actual, overtime, night };
}

function makeRows(year, month) {
  const y = Number(year) || 2026;
  const m = Math.min(12, Math.max(1, Number(month) || 1));
  const last = new Date(y, m, 0).getDate();

  return Array.from({ length: last }, (_, index) => {
    const day = index + 1;
    const d = new Date(y, m - 1, day);
    const key = `${y}-${pad(m)}-${pad(day)}`;

    return {
      id: key,
      date: day,
      week: weekNames[d.getDay()],
      holiday: holidays2026[key] || "",
      transport: "",
      vehicle: false,
      site: "",
      workType: "通常",
      start: "",
      end: "",
      breakBefore22: "",
      breakAfter22: "",
      memo: "",
    };
  });
}

function makeTransferData({ year, month, employeeNo, employeeName, rows }) {
  return {
    appName: "勤務実施表アプリ",
    version: 1,
    exportedAt: new Date().toISOString(),
    year,
    month,
    employeeNo,
    employeeName,
    rows,
  };
}

function validateTransferData(data) {
  if (!data || typeof data !== "object") return false;
  if (!Array.isArray(data.rows)) return false;
  if (!data.year || !data.month) return false;
  return true;
}

function loadSavedState() {
  try {
    const text = safeLocalStorageGet(STORAGE_KEY);
    if (!text) return null;
    const data = JSON.parse(text);
    if (!validateTransferData(data)) return null;
    return data;
  } catch (error) {
    return null;
  }
}

function copyRowsToMonth(sourceRows, year, month) {
  const targetRows = makeRows(year, month);
  return targetRows.map((targetRow, index) => {
    const sourceRow = sourceRows[index];
    if (!sourceRow) return targetRow;
    return {
      ...targetRow,
      transport: sourceRow.transport || "",
      vehicle: Boolean(sourceRow.vehicle),
      site: sourceRow.site || "",
      workType: sourceRow.workType || "通常",
      start: sourceRow.start || "",
      end: sourceRow.end || "",
      breakBefore22: sourceRow.breakBefore22 || "",
      breakAfter22: sourceRow.breakAfter22 || "",
      memo: sourceRow.memo || "",
    };
  });
}

function runTests() {
  const tests = [
    { name: "有休は7時間", actual: calcRow({ workType: "有休" }).actual, expected: 7 },
    { name: "有給も7時間", actual: calcRow({ workType: "有給" }).actual, expected: 7 },
    { name: "公休は0時間", actual: calcRow({ workType: "公休", start: "09:00", end: "18:00" }).actual, expected: 0 },
    { name: "欠勤は0時間", actual: calcRow({ workType: "欠勤", start: "09:00", end: "18:00" }).actual, expected: 0 },
    { name: "9:00〜18:00 休憩1hは実働8時間", actual: calcRow({ workType: "通常", start: "09:00", end: "18:00", breakBefore22: "1", breakAfter22: "0" }).actual, expected: 8 },
    { name: "9:00〜19:00 休憩1hは残業1時間", actual: calcRow({ workType: "通常", start: "09:00", end: "19:00", breakBefore22: "1", breakAfter22: "0" }).overtime, expected: 1 },
    { name: "22:00〜5:00 深夜休憩1hは深夜6時間", actual: calcRow({ workType: "通常", start: "22:00", end: "05:00", breakBefore22: "0", breakAfter22: "1" }).night, expected: 6 },
    { name: "30分休憩は0.5時間", actual: calcRow({ workType: "通常", start: "09:00", end: "18:00", breakBefore22: "0.5", breakAfter22: "0" }).actual, expected: 8.5 },
    { name: "2026年1月は31日分作成", actual: makeRows(2026, 1).length, expected: 31 },
    { name: "2026年2月は28日分作成", actual: makeRows(2026, 2).length, expected: 28 },
    { name: "転送データはrowsを含む", actual: makeTransferData({ year: 2026, month: 1, employeeNo: "1", employeeName: "テスト", rows: makeRows(2026, 1) }).rows.length, expected: 31 },
    { name: "翌月コピーは日数を変更できる", actual: copyRowsToMonth(makeRows(2026, 1), 2026, 2).length, expected: 28 },
  ];

  return tests.map((test) => ({
    ...test,
    passed: Math.abs(test.actual - test.expected) < 0.0001,
  }));
}

function csvEscape(value) {
  return `"${String(value ?? "").replace(/"/g, '""')}"`;
}

function escapeHtml(value) {
  return String(value ?? "")
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;")
    .replace(/"/g, "&quot;");
}

function makeDownload(blob, filename) {
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href = url;
  a.download = filename;
  document.body.appendChild(a);
  a.click();
  a.remove();
  URL.revokeObjectURL(url);
}

function readFileAsText(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => resolve(String(reader.result || ""));
    reader.onerror = () => reject(new Error("ファイルを読み込めませんでした"));
    reader.readAsText(file, "utf-8");
  });
}

function Field({ label, children }) {
  return (
    <label className="block">
      <span className="mb-1 block text-xs font-medium text-slate-500">{label}</span>
      {children}
    </label>
  );
}

function AppButton({ children, onClick, variant = "primary" }) {
  const base = "rounded-xl px-4 py-2 text-sm font-semibold shadow-sm transition active:scale-[0.99]";
  const style = variant === "secondary" ? "border border-slate-300 bg-white text-slate-800 hover:bg-slate-50" : "bg-slate-900 text-white hover:bg-slate-700";
  return (
    <button type="button" onClick={onClick} className={`${base} ${style}`}>
      {children}
    </button>
  );
}

const MobileDayCard = memo(function MobileDayCard({ row, updateRow }) {
  const [siteText, setSiteText] = useState(row.site || "");
  const [memoText, setMemoText] = useState(row.memo || "");

  useEffect(() => {
    setSiteText(row.site || "");
  }, [row.site]);

  useEffect(() => {
    setMemoText(row.memo || "");
  }, [row.memo]);

  const c = calcRow(row);
  const isHoliday = row.week === "日" || Boolean(row.holiday);
  const isSaturday = row.week === "土";
  const borderClass = isHoliday ? "border-rose-200 bg-rose-50" : isSaturday ? "border-sky-200 bg-sky-50" : "border-slate-200 bg-white";

  const commitSite = () => {
    if (siteText !== row.site) updateRow(row.id, "site", siteText);
  };

  const commitMemo = () => {
    if (memoText !== row.memo) updateRow(row.id, "memo", memoText);
  };

  return (
    <div className={`rounded-2xl border p-4 shadow-sm ${borderClass}`}>
      <div className="mb-3 flex items-center justify-between">
        <div className="text-lg font-bold">{row.date}日（{row.week}）</div>
        <div className="text-xs text-rose-700">{row.holiday}</div>
      </div>
      <div className="grid gap-3">
        <Field label="区分">
          <select className={inputClass} value={row.workType} onChange={(e) => updateRow(row.id, "workType", e.target.value)}>
            {workTypes.map((type) => <option key={type} value={type}>{type}</option>)}
          </select>
        </Field>
        <Field label="現場名">
          <input
            className={inputClass}
            value={siteText}
            onChange={(e) => setSiteText(e.target.value)}
            onBlur={commitSite}
            onKeyDown={(e) => { if (e.key === "Enter") e.currentTarget.blur(); }}
            placeholder="現場名"
          />
        </Field>
        <div className="grid grid-cols-2 gap-3">
          <Field label="始業">
            <select className={inputClass} value={row.start} onChange={(e) => updateRow(row.id, "start", e.target.value)}>
              <option value="">選択</option>
              {timeOptions.map((time) => <option key={time} value={time}>{time}</option>)}
            </select>
          </Field>
          <Field label="終業">
            <select className={inputClass} value={row.end} onChange={(e) => updateRow(row.id, "end", e.target.value)}>
              <option value="">選択</option>
              {timeOptions.map((time) => <option key={time} value={time}>{time}</option>)}
            </select>
          </Field>
        </div>
        <div className="grid grid-cols-2 gap-3">
          <Field label="22時以前休憩">
            <select className={inputClass} value={row.breakBefore22} onChange={(e) => updateRow(row.id, "breakBefore22", e.target.value)}>
              <option value="">選択</option>
              {breakOptions.map((time) => <option key={time} value={time}>{time}h</option>)}
            </select>
          </Field>
          <Field label="22時以降休憩">
            <select className={inputClass} value={row.breakAfter22} onChange={(e) => updateRow(row.id, "breakAfter22", e.target.value)}>
              <option value="">選択</option>
              {breakOptions.map((time) => <option key={time} value={time}>{time}h</option>)}
            </select>
          </Field>
        </div>
        <div className="grid grid-cols-2 gap-3">
          <Field label="交通費">
            <input className={inputClass} type="number" value={row.transport} onChange={(e) => updateRow(row.id, "transport", e.target.value)} />
          </Field>
          <label className="mt-6 flex items-center gap-2 text-sm font-medium text-slate-700">
            <input type="checkbox" checked={row.vehicle} onChange={(e) => updateRow(row.id, "vehicle", e.target.checked)} />車両あり
          </label>
        </div>
        <Field label="備考">
          <input
            className={inputClass}
            value={memoText}
            onChange={(e) => setMemoText(e.target.value)}
            onBlur={commitMemo}
            onKeyDown={(e) => { if (e.key === "Enter") e.currentTarget.blur(); }}
            placeholder="備考"
          />
        </Field>
        <div className="rounded-xl bg-white/70 p-3 text-sm">
          実働 <span className="font-bold">{c.actual.toFixed(2)}h</span>　残業 {c.overtime.toFixed(2)}h　深夜 {c.night.toFixed(2)}h
        </div>
      </div>
    </div>
  );
});

export default function WorkReportApp() {
  const savedState = loadSavedState();
  const [year, setYear] = useState(savedState?.year || 2026);
  const [month, setMonth] = useState(savedState?.month || 1);
  const [employeeNo, setEmployeeNo] = useState(savedState?.employeeNo || "18");
  const [employeeName, setEmployeeName] = useState(savedState?.employeeName || "相原孝人");
  const [rows, setRows] = useState(() => savedState?.rows || makeRows(2026, 1));
  const [viewMode, setViewMode] = useState("mobile");
  const [lastSavedAt, setLastSavedAt] = useState("");

  useEffect(() => {
    const data = makeTransferData({ year, month, employeeNo, employeeName, rows });
    safeLocalStorageSet(STORAGE_KEY, JSON.stringify(data));
    setLastSavedAt(new Date().toLocaleTimeString("ja-JP", { hour: "2-digit", minute: "2-digit", second: "2-digit" }));
  }, [year, month, employeeNo, employeeName, rows]);

  const resetMonth = (nextYear, nextMonth) => {
    const y = Number(nextYear) || 2026;
    const m = Math.min(12, Math.max(1, Number(nextMonth) || 1));
    setYear(y);
    setMonth(m);
    setRows(makeRows(y, m));
  };

  const changeMonthKeepingPattern = (nextYear, nextMonth) => {
    const y = Number(nextYear) || 2026;
    const m = Math.min(12, Math.max(1, Number(nextMonth) || 1));
    setYear(y);
    setMonth(m);
    setRows(copyRowsToMonth(rows, y, m));
  };

  const copyCurrentMonthToNextMonth = () => {
    const nextMonth = month === 12 ? 1 : month + 1;
    const nextYear = month === 12 ? year + 1 : year;
    changeMonthKeepingPattern(nextYear, nextMonth);
  };

  const clearSavedData = () => {
    if (!window.confirm("この端末に保存された入力内容を消して、新しい月の空データに戻しますか？")) return;
    safeLocalStorageRemove(STORAGE_KEY);
    setRows(makeRows(year, month));
  };

  const updateRow = (id, key, value) => {
    setRows((prev) => prev.map((row) => (row.id === id ? { ...row, [key]: value } : row)));
  };

  const totals = useMemo(() => {
    return rows.reduce(
      (acc, row) => {
        const c = calcRow(row);
        const isOff = ["公休", "欠勤", "有休", "有給"].includes(row.workType);
        acc.actual += c.actual;
        acc.overtime += c.overtime;
        acc.night += c.night;
        acc.transport += toNumber(row.transport);
        acc.paid += row.workType === "有休" || row.workType === "有給" ? 1 : 0;
        acc.publicOff += row.workType === "公休" ? 1 : 0;
        acc.absent += row.workType === "欠勤" ? 1 : 0;
        acc.workingDays += !isOff && (row.site || row.start || row.end) ? 1 : 0;
        return acc;
      },
      { actual: 0, overtime: 0, night: 0, transport: 0, paid: 0, publicOff: 0, absent: 0, workingDays: 0 }
    );
  }, [rows]);

  const tests = useMemo(() => runTests(), []);
  const allTestsPassed = tests.every((test) => test.passed);
  const header = ["日付", "曜日", "祝日", "交通費", "車両", "現場名", "区分", "始業", "終業", "22時以前休憩", "22時以降深夜休憩", "実働", "残業", "深夜", "メモ"];

  const exportRows = rows.map((row) => {
    const c = calcRow(row);
    return [
      `${year}/${month}/${row.date}`,
      row.week,
      row.holiday,
      row.transport,
      row.vehicle ? "会社車" : "",
      row.site,
      row.workType,
      row.start,
      row.end,
      row.breakBefore22,
      row.breakAfter22,
      c.actual.toFixed(2),
      c.overtime.toFixed(2),
      c.night.toFixed(2),
      row.memo,
    ];
  });

  const downloadCsv = () => {
    const csvText = [header, ...exportRows].map((line) => line.map(csvEscape).join(",")).join(NEW_LINE);
    const blob = new Blob([BOM + csvText], { type: "text/csv;charset=utf-8" });
    makeDownload(blob, `勤務実施表_${year}_${pad(month)}_${employeeName || "未入力"}.csv`);
  };

  const downloadExcel = () => {
    const summaryRows = [
      ["社員NO.", employeeNo, "氏名", employeeName, "対象年月", `${year}年${month}月`],
      ["稼働日数", totals.workingDays, "有給日数", totals.paid, "公休日数", totals.publicOff, "欠勤日数", totals.absent],
      ["時間数計", totals.actual.toFixed(2), "残業", totals.overtime.toFixed(2), "深夜", totals.night.toFixed(2), "交通費", totals.transport],
    ];

    const summaryHtml = summaryRows.map((line) => `<tr>${line.map((cell) => `<td>${escapeHtml(cell)}</td>`).join("")}</tr>`).join("");
    const headerHtml = `<tr>${header.map((h) => `<th style="background:#dbeafe;">${escapeHtml(h)}</th>`).join("")}</tr>`;
    const bodyHtml = exportRows.map((line) => `<tr>${line.map((cell) => `<td>${escapeHtml(cell)}</td>`).join("")}</tr>`).join("");

    const html = [
      "<!DOCTYPE html>",
      "<html>",
      "<head>",
      "<meta charset=\"UTF-8\" />",
      "</head>",
      "<body>",
      "<table border=\"1\">",
      "<tr><th colspan=\"15\" style=\"font-size:18px;background:#e5e7eb;\">勤務実施表</th></tr>",
      summaryHtml,
      headerHtml,
      bodyHtml,
      "</table>",
      "</body>",
      "</html>",
    ].join(NEW_LINE);

    const blob = new Blob([BOM + html], { type: "application/vnd.ms-excel;charset=utf-8" });
    makeDownload(blob, `勤務実施表_${year}_${pad(month)}_${employeeName || "未入力"}.xls`);
  };

  const downloadTransferData = () => {
    const data = makeTransferData({ year, month, employeeNo, employeeName, rows });
    const jsonText = JSON.stringify(data, null, 2);
    const blob = new Blob([BOM + jsonText], { type: "application/json;charset=utf-8" });
    makeDownload(blob, `勤務実施表_入力データ_${year}_${pad(month)}_${employeeName || "未入力"}.json`);
  };

  const importTransferData = async (event) => {
    const file = event.target.files && event.target.files[0];
    if (!file) return;

    try {
      const text = await readFileAsText(file);
      const cleanedText = text.replace(/^\uFEFF/, "");
      const data = JSON.parse(cleanedText);

      if (!validateTransferData(data)) {
        alert("勤務実施表の入力データではないようです。");
        return;
      }

      setYear(Number(data.year) || 2026);
      setMonth(Math.min(12, Math.max(1, Number(data.month) || 1)));
      setEmployeeNo(String(data.employeeNo || ""));
      setEmployeeName(String(data.employeeName || ""));
      setRows(data.rows);
      setViewMode("table");
      alert("入力データを読み込みました。PCでExcel出力できます。");
    } catch (error) {
      alert("読み込みに失敗しました。スマホで出力したJSONファイルを選んでください。");
    } finally {
      event.target.value = "";
    }
  };

  const renderTableRows = () => rows.map((row) => {
    const c = calcRow(row);
    const rowClass = row.week === "日" || row.holiday ? "bg-rose-50" : row.week === "土" ? "bg-sky-50" : "bg-white";
    return (
      <tr key={row.id} className={rowClass}>
        <td className="border-b border-slate-100 p-2 font-medium">{row.date}</td>
        <td className="border-b border-slate-100 p-2">{row.week}</td>
        <td className="border-b border-slate-100 p-2 text-xs text-rose-700">{row.holiday}</td>
        <td className="border-b border-slate-100 p-1"><input className={inputClass} type="number" value={row.transport} onChange={(e) => updateRow(row.id, "transport", e.target.value)} /></td>
        <td className="border-b border-slate-100 p-2 text-center"><input type="checkbox" checked={row.vehicle} onChange={(e) => updateRow(row.id, "vehicle", e.target.checked)} /></td>
        <td className="border-b border-slate-100 p-1"><input className={inputClass} value={row.site} onChange={(e) => updateRow(row.id, "site", e.target.value)} placeholder="現場名" /></td>
        <td className="border-b border-slate-100 p-1"><select className={inputClass} value={row.workType} onChange={(e) => updateRow(row.id, "workType", e.target.value)}>{workTypes.map((type) => <option key={type} value={type}>{type}</option>)}</select></td>
        <td className="border-b border-slate-100 p-1"><select className={inputClass} value={row.start} onChange={(e) => updateRow(row.id, "start", e.target.value)}><option value="">選択</option>{timeOptions.map((time) => <option key={time} value={time}>{time}</option>)}</select></td>
        <td className="border-b border-slate-100 p-1"><select className={inputClass} value={row.end} onChange={(e) => updateRow(row.id, "end", e.target.value)}><option value="">選択</option>{timeOptions.map((time) => <option key={time} value={time}>{time}</option>)}</select></td>
        <td className="border-b border-slate-100 p-1"><select className={inputClass} value={row.breakBefore22} onChange={(e) => updateRow(row.id, "breakBefore22", e.target.value)}><option value="">選択</option>{breakOptions.map((time) => <option key={time} value={time}>{time}h</option>)}</select></td>
        <td className="border-b border-slate-100 p-1"><select className={inputClass} value={row.breakAfter22} onChange={(e) => updateRow(row.id, "breakAfter22", e.target.value)}><option value="">選択</option>{breakOptions.map((time) => <option key={time} value={time}>{time}h</option>)}</select></td>
        <td className="border-b border-slate-100 p-2 font-semibold">{c.actual.toFixed(2)}</td>
        <td className="border-b border-slate-100 p-2">{c.overtime.toFixed(2)}</td>
        <td className="border-b border-slate-100 p-2">{c.night.toFixed(2)}</td>
        <td className="border-b border-slate-100 p-1"><input className={inputClass} value={row.memo} onChange={(e) => updateRow(row.id, "memo", e.target.value)} placeholder="備考" /></td>
      </tr>
    );
  });

  return (
    <div className="min-h-screen bg-slate-50 p-4 text-slate-900">
      <div className="mx-auto max-w-7xl space-y-4">
        <header className="rounded-2xl bg-white p-5 shadow-sm">
          <div className="flex flex-col gap-4 md:flex-row md:items-center md:justify-between">
            <div>
              <h1 className="text-2xl font-bold tracking-tight">勤務実施表アプリ</h1>
              <p className="mt-1 text-sm text-slate-500">自動保存・スマホ入力・月コピー・Excel様式出力に対応した試作品です。</p>
              <p className="mt-1 text-xs text-emerald-700">自動保存済み：{lastSavedAt || "未保存"}</p>
            </div>
            <div className="flex flex-wrap gap-2">
              <AppButton onClick={downloadExcel}>Excel出力</AppButton>
              <AppButton variant="secondary" onClick={downloadTransferData}>入力データ出力</AppButton>
              <label className="cursor-pointer rounded-xl border border-slate-300 bg-white px-4 py-2 text-sm font-semibold text-slate-800 shadow-sm hover:bg-slate-50">
                入力データ読込
                <input type="file" accept=".json,application/json" onChange={importTransferData} className="hidden" />
              </label>
              <AppButton variant="secondary" onClick={downloadCsv}>CSV出力</AppButton>
              <AppButton variant="secondary" onClick={() => window.print()}>印刷</AppButton>
              <AppButton variant="secondary" onClick={copyCurrentMonthToNextMonth}>翌月へコピー</AppButton>
              <AppButton variant="secondary" onClick={clearSavedData}>保存データ削除</AppButton>
            </div>
          </div>
        </header>

        <section className="rounded-2xl bg-white p-3 shadow-sm">
          <div className="flex flex-wrap gap-2">
            <AppButton variant={viewMode === "mobile" ? "primary" : "secondary"} onClick={() => setViewMode("mobile")}>スマホ入力画面</AppButton>
            <AppButton variant={viewMode === "table" ? "primary" : "secondary"} onClick={() => setViewMode("table")}>一覧表画面</AppButton>
            <AppButton variant={viewMode === "excel" ? "primary" : "secondary"} onClick={() => setViewMode("excel")}>Excel様式確認</AppButton>
          </div>
        </section>

        <section className="grid gap-4 md:grid-cols-4">
          <div className="rounded-2xl bg-white p-4 shadow-sm md:col-span-2">
            <div className="grid gap-3 md:grid-cols-4">
              <Field label="年"><input className={inputClass} type="number" value={year} onChange={(e) => resetMonth(e.target.value, month)} /></Field>
              <Field label="月">
                <select className={inputClass} value={String(month)} onChange={(e) => resetMonth(year, e.target.value)}>
                  {Array.from({ length: 12 }, (_, index) => {
                    const value = String(index + 1);
                    return <option key={value} value={value}>{index + 1}月</option>;
                  })}
                </select>
              </Field>
              <Field label="社員NO."><input className={inputClass} value={employeeNo} onChange={(e) => setEmployeeNo(e.target.value)} /></Field>
              <Field label="氏名"><input className={inputClass} value={employeeName} onChange={(e) => setEmployeeName(e.target.value)} /></Field>
            </div>
          </div>

          <div className="rounded-2xl bg-white p-4 shadow-sm">
            <div className="text-sm font-medium text-slate-500">時間数計</div>
            <div className="mt-1 text-3xl font-bold">{totals.actual.toFixed(2)} h</div>
            <div className="mt-1 text-xs text-slate-500">残業 {totals.overtime.toFixed(2)}h / 深夜 {totals.night.toFixed(2)}h</div>
          </div>

          <div className="rounded-2xl bg-white p-4 shadow-sm">
            <div className="text-sm font-medium text-slate-500">日数・交通費</div>
            <div className="mt-1 text-3xl font-bold">{totals.workingDays} 日</div>
            <div className="mt-1 text-xs text-slate-500">有給 {totals.paid}日 / 公休 {totals.publicOff}日 / 欠勤 {totals.absent}日 / 交通費 {totals.transport.toLocaleString()}円</div>
          </div>
        </section>

        {viewMode === "mobile" && <section className="grid gap-4 md:hidden">{rows.map((row) => <MobileDayCard key={row.id} row={row} updateRow={updateRow} />)}</section>}
        {viewMode === "mobile" && <section className="hidden rounded-2xl bg-white p-4 text-sm text-slate-600 shadow-sm md:block">スマホ入力画面はスマホ幅で見やすいカード形式です。PCでは「一覧表画面」または「Excel様式確認」も使えます。</section>}

        {viewMode === "table" && (
          <section className="rounded-2xl bg-white shadow-sm">
            <div className="overflow-x-auto">
              <table className="w-full min-w-[1180px] border-collapse text-sm">
                <thead className="bg-slate-100">
                  <tr>{["日", "曜", "祝日", "交通費", "車両", "現場名", "区分", "始業", "終業", "22時以前休憩", "22時以降休憩", "実働", "残業", "深夜", "メモ"].map((heading) => <th key={heading} className="border-b border-slate-200 p-2 text-left font-semibold">{heading}</th>)}</tr>
                </thead>
                <tbody>{renderTableRows()}</tbody>
              </table>
            </div>
          </section>
        )}

        {viewMode === "excel" && (
          <section className="rounded-2xl bg-white p-4 shadow-sm">
            <div className="mb-3 text-sm font-semibold text-slate-700">Excel様式確認プレビュー</div>
            <div className="overflow-x-auto">
              <table className="min-w-[1000px] border-collapse text-xs">
                <tbody>
                  <tr><td colSpan={15} className="border bg-slate-200 p-2 text-center text-lg font-bold">勤務実施表</td></tr>
                  <tr><td className="border p-1 font-semibold">社員NO.</td><td className="border p-1">{employeeNo}</td><td className="border p-1 font-semibold">氏名</td><td className="border p-1">{employeeName}</td><td className="border p-1 font-semibold">対象年月</td><td className="border p-1">{year}年{month}月</td><td colSpan={9} className="border p-1"></td></tr>
                  <tr>{header.map((h) => <td key={h} className="border bg-blue-100 p-1 font-semibold">{h}</td>)}</tr>
                  {exportRows.map((line, index) => <tr key={index}>{line.map((cell, cellIndex) => <td key={cellIndex} className="border p-1">{cell}</td>)}</tr>)}
                  <tr><td colSpan={15} className="border bg-slate-100 p-1 font-semibold">集計</td></tr>
                  <tr><td className="border p-1">稼働日数</td><td className="border p-1">{totals.workingDays}</td><td className="border p-1">有給日数</td><td className="border p-1">{totals.paid}</td><td className="border p-1">公休日数</td><td className="border p-1">{totals.publicOff}</td><td className="border p-1">欠勤日数</td><td className="border p-1">{totals.absent}</td><td className="border p-1">時間数計</td><td className="border p-1">{totals.actual.toFixed(2)}</td><td className="border p-1">残業</td><td className="border p-1">{totals.overtime.toFixed(2)}</td><td className="border p-1">深夜</td><td className="border p-1">{totals.night.toFixed(2)}</td><td className="border p-1">交通費 {totals.transport}</td></tr>
                </tbody>
              </table>
            </div>
          </section>
        )}

        <section className="grid gap-4 md:grid-cols-2">
          <div className="rounded-2xl bg-white p-4 text-sm text-slate-600 shadow-sm">
            <div className="mb-1 font-semibold text-slate-800">この試作品で再現済みの内容</div>
            <p>自動保存、スマホ専用入力画面、翌月コピー、月別の日付自動作成、土曜・日祝の色分け、有休/有給は実働7時間、通常勤務の実働・残業・深夜勤務、交通費、有給・公休・欠勤日数の集計、Excel様式プレビュー、Excel出力、CSV出力、スマホからPCへ渡すための入力データ出力/読込。</p>
            <div className="mt-3 rounded-xl bg-slate-50 p-3 text-xs text-slate-700">
              <div className="font-semibold">手動送付の使い方</div>
              <p className="mt-1">スマホで入力後「入力データ出力」を押してJSONファイルを保存し、メール・LINE WORKS・USB等でPCへ送ります。PC側でこの画面を開き「入力データ読込」を押してJSONを選び、その後「Excel出力」を押します。</p>
            </div>
          </div>

          <div className="rounded-2xl bg-white p-4 text-sm shadow-sm">
            <div className="mb-2 font-semibold text-slate-800">計算テスト：{allTestsPassed ? "全てOK" : "要確認"}</div>
            <ul className="space-y-1">
              {tests.map((test) => (
                <li key={test.name} className={test.passed ? "text-emerald-700" : "text-rose-700"}>
                  {test.passed ? "✓" : "×"} {test.name}（結果 {test.actual} / 期待 {test.expected}）
                </li>
              ))}
            </ul>
          </div>
        </section>
      </div>
    </div>
  );
}
