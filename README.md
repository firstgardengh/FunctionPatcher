# FunctionPatcher

This function takes a function's pointer, sets the memory protection to `PAGE_READWRITE`, casts it to a `uintptr_t`, sets the value to `0` and reverts the protection.


```cpp
uintptr_t original_value;

void function_patch(void* ctx, bool unpatch = false)
{
    DWORD old;

    if (unpatch)
    {
        VirtualProtect(ctx, sizeof(ctx), PAGE_READWRITE, &old);
        *(uintptr_t*)ctx = original_value;
        VirtualProtect(ctx, sizeof(ctx), old, &old);
    }
    else
    {
        original_value = *(uintptr_t*)ctx;
        VirtualProtect(ctx, sizeof(ctx), PAGE_READWRITE, &old);
        *(uintptr_t*)ctx = 0;
        VirtualProtect(ctx, sizeof(ctx), old, &old);
    }
}
```

Parameters:
`ctx`: A pointer to the function to be patched.\n
`unpatch`: A boolean flag indicating whether to unpatch (`true`) or patch (`false`) the function.



Use case would be for specific functions that promote reverse-engineering.
