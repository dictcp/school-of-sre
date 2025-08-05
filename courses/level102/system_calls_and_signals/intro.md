# 系統呼叫與訊號

## 預備知識

- [Linux 基礎](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/)
- [Python 基礎](https://linkedin.github.io/school-of-sre/level101/python_web/intro/)

## 這門課程的預期學習成果

本課程涵蓋訊號與系統呼叫的基本理解。闡明訊號與系統呼叫的知識對 SRE 的幫助。

## 課程未涵蓋內容

本課程不討論訊號以外的其他中斷或中斷處理。課程不會深入探討訊號處理函式與 GNU C 函式庫。

## 課程內容
- [訊號](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/signals)
    - [中斷與訊號簡介](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/signals/#introduction-to-interrupts-and-signals)
    - [訊號類型](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/signals/#types-of-signals)
    - [傳送訊號給程序](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/signals/#sending-signals-to-process)
    - [訊號處理](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/signals/#handling-signals)
    - [訊號在系統呼叫的角色，以 *wait()* 為例](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/signals/#role-of-signals-in-system-calls-with-the-example-of-wait)
- [系統呼叫](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/system_calls)
    - [簡介](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/system_calls/#introduction)
    - [系統呼叫類型](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/system_calls/#types-of-system-calls)
    - [使用者模式、核心模式及其切換](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/system_calls/#user-mode-kernel-mode-and-their-transitions)
    - [*write()* 系統呼叫運作原理](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/system_calls/#working-of-write-system-call)
    - [使用 strace 於 Linux 除錯](https://linkedin.github.io/school-of-sre/level102/system_calls_and_signals/system_calls/#debugging-in-linux-with-strace)
