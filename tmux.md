### Basic concepts
```
Session: A complete environment including windows and panels
Window: It's like a separate terminal (actually a tab)
Pane: Parts of a window (a window can be divided into multiple panes)
```
### The default control key in tmux is Ctrl+b
### Start and manage `session`
```
tmux                   # شروع یک سشن جدید
tmux new -s mysession  # ساخت سشن با نام مشخص
tmux ls                # لیست سشن‌های موجود
tmux attach            # وصل شدن به آخرین سشن
tmux attach -t mysession # وصل شدن به یک سشن خاص
tmux kill-session -t mysession # بستن یک سشن خاص
tmux kill-server # بستن تمام سشن ها
```
###  Working with `Windows`
```
داخل tmux:
Ctrl+b c → ساخت پنجره جدید

Ctrl+b n → رفتن به پنجره بعدی

Ctrl+b p → رفتن به پنجره قبلی

Ctrl+b , → تغییر نام پنجره

Ctrl+b & → بستن پنجره فعلی

Ctrl+b w → نمایش لیست پنجره ها داخل یک سشن

tmux list-windows -t mysession → نمایش لیست پنجره ها بیرون یک سشن
```
### Working with `Panes`
```
Ctrl+b " → تقسیم افقی

Ctrl+b % → تقسیم عمودی

Ctrl+b o → سوییچ بین پنل‌ها

Ctrl+b x → بستن پنل فعلی

Ctrl+b { → جابجا کردن پنل

Ctrl+b q → نمایش شماره پنل ها

Ctrl+b ← / → / ↑ / ↓ → رفتن به پنل دیگر با کلیدهای جهت
```
### Ctrl+b d → detach from session (without closing)
