# Repro case for JNI_OnLoad with GameActivity

This repro case shows how it's awkward to initialize GameActivity when using a
visibility restricted app with `JNI_OnLoad` (which is what we recommend all apps
that are able do).

The fix for this on the app developer's side isn't complicated, they just need
to add one more symbol to their version script. Discovering that is annoying
though, because the requirement isn't documented, and it would be better to keep
the symbol hidden anyway.

Instead, GameActivity should define it's own (weak) `JNI_OnLoad` that calls
`GameActivity_OnLoad`, and apps that use JNI_OnLoad can call
`GameActivity_OnLoad` in their own `JNI_OnLoad`.