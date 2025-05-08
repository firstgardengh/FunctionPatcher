# FunctionPatcher

This function takes a function's pointer, sets the memory protection to PAGE_READWRITE, casts it to a uintptr_t, sets the value to 0 and reverts the protection.

```cpp
void function_patch(void* ctx)
{
      DWORD old;
      VirtualProtect(ctx, sizeof(ctx), PAGE_READWRITE, &old);
      *(uintptr_t*)funct = 0;
      VirtualProtect(funct, sizeof(funct), old, &old);
}

Use case would be for specific functions that promote reverse-engineering.
