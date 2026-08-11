+++
date = '2026-08-11T05:14:41-04:00'
draft = false
title = 'Loading OpenGL functions from scratch with Zig'
+++

Zig metaprogramming is cool and awesome.

It's also useful.

## The Problem

I'm building a desktop application from scratch and I want to do hardware rendering with OpenGL.

OpenGL is not a typical library. It can't be statically linked, nor can it be shipped as a dynamic library. OpenGL is an API specification that different vendors provide an implementation for. 

To actually call OpenGL functions, they have to be loaded at runtime. There are libraries like [GLEW](https://glew.sourceforge.net/) or [GLAD](https://github.com/Dav1dde/glad) that can do this automatically, but I'm not using them because I'm trying to keep dependecies as low as possible.

To load an OpenGL function manually on Windows I can call `wglGetProcAddress`[^1] with the name of the function and it will return a pointer to the function. I just have to write the declaration for each function pointer and then load them at runtime.

```zig
// This will store the function pointer.
pub var Clear: *const fn (mask: u32) callconv(.c) void = undefined;
// rest of the functions...

// The '!' before void means that it can return either an error or void.
pub fn load_functions() !void {
    // If the wglGetProcAddress call returns null we return an error.
    Clear = @ptrCast(platform.gl_get_proc_address("glClear") orelse {
        return error.Function_Not_Found;
    });
    // rest of the functions...
}
```

 > The `platform.gl_get_proc_address` function wraps the call to `wglGetProcAddress` with some extra steps[^2].

But that's a lot of typing. Won't be cool and awesome if I could loop through every declaration in a struct, check if its a function pointer, turn the name of the declaration into a string, attach `gl` in front of it, and pass it into `gl_get_proc_address`. 

Not something you can do without complicated macros or code generation, right?

## The Solution

See if you can understand the following code:

```zig
pub const gl = struct {
    pub var Clear: *const fn (mask: u32) callconv(.c) void = undefined;
    // rest of the functions...
    
    pub const COLOR_BUFFER_BIT = 0x00004000;
    // rest of the constants...
};

fn load_all_functions() !void {
    inline for (@typeInfo(gl).@"struct".decls) |declaration| {
        const info = @typeInfo(@TypeOf(@field(gl, declaration.name)));
        if (info == .pointer and @typeInfo(info.pointer.child) == .@"fn") {
            const full_name = comptime std.fmt.comptimePrint(
                "gl{s}",
                .{declaration.name},
            );
            @field(gl, declaration.name) = @ptrCast(
                platform.gl_get_proc_address(full_name) orelse {
                    return error.Function_Not_Found;
                },
            );
        }
    }
}
```

[Zig](https://ziglang.org) lets you inspect and manipulate types at compile time. This is Zig code that does exactly what I described above. Let's go through it line by line.

```zig
inline for (@typeInfo(gl).@"struct".decls) |declaration| {
```

This loops through every declaration in the struct. The `inline for` unrolls the loop at compile time so that I can work with types inside it. 

[`@typeInfo`](https://ziglang.org/documentation/0.16.0/#typeInfo) is a Zig [builtin function](https://ziglang.org/documentation/0.16.0/#Builtin-Functions) that takes a type and returns a union containing information about the type. I am accessing its `@"struct"` field to get information about the struct, which has a `decls` field containing a list of declarations.

```zig
// declaration.name gives me the name as a string
const info = @typeInfo(@TypeOf(@field(gl, declaration.name)));
```

For each declaration I need to get its type information. I can use the [`@field`](https://ziglang.org/documentation/0.16.0/#field) builtin to access the declaration by name. Then I can find its type with [`@TypeOf`](https://ziglang.org/documentation/0.16.0/#TypeOf) and use `@typeInfo` again to get the the union containing type information. 

        
```zig
if (info == .pointer and @typeInfo(info.pointer.child) == .@"fn") {
```

Then I check declaration's type info union to see if its a pointer and if the type its pointing to is a function. I'm using the `==` operator to check which field of a union is active. 

```zig
const full_name = comptime std.fmt.comptimePrint(
    "gl{s}",
    .{declaration.name},
);
```

Here I'm formatting a string at compile time. I take the name of the declaration, lets say `"Clear"` for example, and attach `"gl"` to the front to make `"glClear"`. 


```zig
@field(gl, declaration.name) = @ptrCast(
    platform.gl_get_proc_address(full_name) orelse {
        return error.Function_Not_Found;
    },
);
```
This code passes the string to `gl_get_proc_address` which returns a `?*anyopaque` (equivalent to `void*` in C or `rawptr` in Odin). The `?` means that the type is nullable. 

To get a useable value, I can unwraped it with `orelse` giving me a `*anyopaque`. If the `?*anyopaque` was null during the unwrap we return an error. 

The `@ptrCast` takes the resulting `*anyopaque`  and casts it to the type of the declaration. 

I used `@field` earlier to access a value by name, but it can also be used to set a value. Here, I'm using it to assign the function pointer to the decleration in `gl`. 

Now I can define more OpenGL functions and start using them.

``` zig
pub const gl = struct {
    pub var ClearColor: *const fn // omitted
    pub var Clear: *const fn      // for
    pub var Viewport: *const fn   // brevity

    pub const COLOR_BUFFER_BIT = 0x00004000;
};

pub fn main() !void {
    // set up window and OpenGL context
    try load_all_functions();
    while (app_running) {
        // poll events
        
        gl.Viewport(0, 0, window.width, window.height);
        gl.ClearColor(0.39, 0.58, 0.92, 1.0);
        gl.Clear(gl.COLOR_BUFFER_BIT);
    
        // swap buffers
    } 
}
```

## Cool and Awesome
I only started writing in Zig a few days ago and that didn't stop me from picking up its metaprogramming features. Not because I have some sort of special ability to understand it, but because it's simple, well designed, and easy to use.

I hope you give [Zig](https://ziglang.org/) a try.

> This article was handwritten by me, if you notice any errors please direct them to [my email](mailto:novelalex29@outlook.com).

[^1]: I'm oversimplifying to focus on the topic at hand. For the details see: <https://wikis.khronos.org/opengl/Creating_an_OpenGL_Context_(WGL)>.

[^2]: The extra steps: <https://wikis.khronos.org/opengl/Load_OpenGL_Functions#Windows>.
