# Shaiya Episode 6 - Server

A library that modifies ps_game to make it compatible with Episode 6 clients.

## Getting Started

1. Navigate to the `bin` directory and read the documentation. Install the binaries to `SERVER\PSM_Client\Bin` and execute the store procedures in SQL Server Management Studio.

2. Open the project in Visual Studio, target the x86 platform, and build the solution. Copy the library to `SERVER\PSM_Client\Bin` and use `ps_game.ct` to inject the code.

3. The game service is owned by the `NT AUTHORITY\SYSTEM` account. When the library attempts to establish a trusted connection with SQL Server, it will try to log in with that account. One way to ensure the login succeeds is to add `NT AUTHORITY\SYSTEM` to the `sysadmin` role.

```sql
ALTER SERVER ROLE [sysadmin] ADD MEMBER [NT AUTHORITY\SYSTEM]

```

## Contributors

Everyone is welcome to submit a pull request or open an issue. Whether or not the code is merged, your time and effort is respected and appreciated.

### Guidelines

#### C++

Most expressions in the `shaiya` namespace match the debug information, etc. The intent is to keep it that way. Aside from that, here are some suggested naming conventions.

`snake_case`

* namespaces
* constants
* local variables
* global variables
* functions
* member methods

`camelCase`

* parameters
* structure fields
* member variables

`PascalCase`

* type definitions
* structures
* classes


#### x86 ASM

Use as little as machine code as possible. Please follow the style in the code block below. It shows the address of the detour and where it returns without having to look elsewhere.

```
unsigned u0x47A00C = 0x47A00C;
void __declspec(naked) naked_0x47A003()
{
    __asm
    {
        pushad

        push esi // packet
        push edi // user
        call packet_gem::item_compose_handler
        add esp,0x8
        
        popad

        jmp u0x47A00C
    }
}
```

#### .NET

The source and headers contain `pragma` directives to tell the compiler whether the code is managed or unmanaged. It will default to managed, so please pay attention to the warnings. For example: the `naked` attribute and `DllMain` will emit a warning.

```
#pragma managed
#pragma unmanaged
```