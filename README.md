# TextInputLayout-MemoryLeak
Tiny app with a reproducible memory leak in a TextInputLayout

## How to reproduce a memory leak
1. Compile and install.
2. Click next, click edit text, press back.
3. Wait for little bit (maybe hide the app to trigger leak canary).
4. See the leak in the logcat.

```
====================================
    HEAP ANALYSIS RESULT
    ====================================
    1 APPLICATION LEAKS
    
    References underlined with "~~~" are likely causes.
    Learn more at https://squ.re/leaks.
    
    1238 bytes retained by leaking objects
    Signature: 7ae23bd2abb2bcea68b932be0c80fb2edb984f
    ┬───
    │ GC Root: System class
    │
    ├─ android.view.inputmethod.InputMethodManager class
    │    Leaking: NO (InputMethodManager↓ is not leaking and a class is never leaking)
    │    ↓ static InputMethodManager.sInstance
    ├─ android.view.inputmethod.InputMethodManager instance
    │    Leaking: NO (InputMethodManager is a singleton)
    │    ↓ InputMethodManager.mCurrentInputConnection
    │                         ~~~~~~~~~~~~~~~~~~~~~~~
    ├─ com.android.internal.widget.EditableInputConnection instance
    │    Leaking: UNKNOWN
    │    ↓ EditableInputConnection.mTargetView
    │                              ~~~~~~~~~~~
    ├─ com.google.android.material.textfield.TextInputEditText instance
    │    Leaking: YES (View detached and has parent)
    │    mContext instance of androidx.appcompat.view.ContextThemeWrapper, wrapping activity com.example.edittextmemoryleak.MainActivity with mDestroyed = false
    │    View#mParent is set
    │    View#mAttachInfo is null (view detached)
    │    View.mID = R.id.password
    │    View.mWindowAttachCount = 1
    │    ↓ TextInputEditText.mParent
    ├─ android.widget.FrameLayout instance
    │    Leaking: YES (TextInputEditText↑ is leaking and View detached and has parent)
    │    mContext instance of androidx.appcompat.view.ContextThemeWrapper, wrapping activity com.example.edittextmemoryleak.MainActivity with mDestroyed = false
    │    View#mParent is set
    │    View#mAttachInfo is null (view detached)
    │    View.mWindowAttachCount = 1
    │    ↓ FrameLayout.mParent
    ├─ com.google.android.material.textfield.TextInputLayout instance
    │    Leaking: YES (FrameLayout↑ is leaking and View detached and has parent)
    │    mContext instance of androidx.appcompat.view.ContextThemeWrapper, wrapping activity com.example.edittextmemoryleak.MainActivity with mDestroyed = false
    │    View#mParent is set
    │    View#mAttachInfo is null (view detached)
    │    View.mID = R.id.password_layout
    │    View.mWindowAttachCount = 1
    │    ↓ TextInputLayout.mParent
    ╰→ android.widget.FrameLayout instance
    ​     Leaking: YES (ObjectWatcher was watching this because com.example.edittextmemoryleak.SecondFragment received Fragment#onDestroyView() callback (references to its views should be cleared to prevent leaks))
    ​     key = dc7f2ee5-b7f1-496f-ab09-54a15dbddfd0
    ​     watchDurationMillis = 37836
    ​     retainedDurationMillis = 32835
    ​     key = 7d5ebcae-c9ac-4ad1-a04e-49808debd845
    ​     mContext instance of com.example.edittextmemoryleak.MainActivity with mDestroyed = false
    ​     View#mParent is null
    ​     View#mAttachInfo is null (view detached)
    ​     View.mWindowAttachCount = 1
    ====================================
    0 LIBRARY LEAKS
    
    Library Leaks are leaks coming from the Android Framework or Google libraries.
    ====================================
    METADATA
    
    Please include this in bug reports and Stack Overflow questions.
    
    Build.VERSION.SDK_INT: 29
    Build.MANUFACTURER: samsung
    LeakCanary version: 2.2
    App process name: com.example.edittextmemoryleak
    Analysis duration: 4424 ms
    Heap dump file path: /data/user/0/com.example.edittextmemoryleak/files/leakcanary/2020-05-11_13-58-18_741.hprof
    Heap dump timestamp: 1589219906373
    ====================================
```
