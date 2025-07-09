# GitHub Actions Workflows for SynapseX

This directory contains optimized GitHub Actions workflows for building SynapseX on the x86/Win32 platform.

### 4. `debug-build-x86.yml` - Debug & Diagnostic Build
- **Purpose**: Troubleshooting and diagnostic build with detailed logging
- **Triggers**: Manual dispatch only (with input parameters)
- **Features**:
  - Configurable verbosity levels (quiet to diagnostic)
  - Optional caching control
  - Environment diagnostics and system info
  - Solution structure analysis
  - Detailed build timing and output analysis
  - Debug artifacts collection

## Workflows

### 1. `build-x86-win32.yml` - Standard Optimized Build
- **Purpose**: Comprehensive x86/Win32 Release build with caching and artifact collection
- **Triggers**: Push/PR to master/main/develop branches, manual dispatch
- **Features**:
  - Multi-processor compilation enabled
  - NuGet package caching
  - MSBuild artifact caching
  - Comprehensive build verification
  - Detailed build artifacts upload
  - Build summary generation

### 2. `fast-build-x86.yml` - Ultra-Fast Build
- **Purpose**: Speed-optimized build for quick validation (15-minute timeout)
- **Triggers**: Push/PR to master/main/develop branches, manual dispatch
- **Features**:
  - Minimal checkout (no LFS)
  - Aggressive caching strategy
  - Quiet build output
  - Critical artifacts only
  - Ultra-optimized MSBuild flags

### 3. `optimized-x86-build.yml` - Advanced Optimized Build
- **Purpose**: Most sophisticated build with advanced caching and performance optimization
- **Triggers**: Push/PR (ignoring docs), manual dispatch
- **Features**:
  - Concurrency control (cancel in-progress builds)
  - Path-based filtering
  - Advanced Visual Studio environment setup
  - Sophisticated caching strategy
  - Detailed performance reporting
  - Comprehensive build validation

## Build Optimizations Applied

### MSBuild Optimizations
- `/maxcpucount` - Use all available CPU cores
- `/p:UseMultiToolTask=true` - Enable multi-tool task execution
- `/p:BuildInParallel=true` - Build projects in parallel
- `/p:PreferredToolArchitecture=x64` - Use 64-bit tools for better performance
- `/p:MultiProcessorCompilation=true` - Enable MP compilation in C++ projects
- `/p:CL_MPCount=0` - Use all available processors for C++ compilation
- `/nodeReuse:false` - Prevent MSBuild node reuse (better for CI)

### C++ Specific Optimizations
- Disabled code analysis for faster builds
- Reduced warning levels where appropriate
- Enabled whole program optimization for Release builds
- Multi-processor compilation enabled

### Caching Strategy
- **NuGet packages**: Cached based on packages.config and project files
- **MSBuild artifacts**: Cached based on source file hashes
- **Build intermediate files**: Cached obj/bin directories
- **Visual Studio cache**: Cached .vs directory

### Environment Optimizations
- Disabled .NET telemetry
- Disabled first-time experience prompts
- Optimized NuGet restore with DirectDownload
- Pre-configured package directories

## Usage

These workflows will automatically trigger on:
- Push to master, main, or develop branches
- Pull requests targeting those branches
- Manual workflow dispatch from GitHub Actions UI

## Artifacts

Each successful build produces:
- **SynapseX-x86-Release-{sha}**: Complete build artifacts (30-day retention)
- **SynapseX-x86-FastBuild-{run_number}**: Critical artifacts only (7-day retention)  
- **SynapseX-x86-Optimized-{run_number}**: Optimized build artifacts (30-day retention)
- **Debug-Build-{config}-{run_number}**: Debug build artifacts with logs (3-day retention)

## Performance Notes

- **Standard build**: ~10-15 minutes (comprehensive)
- **Fast build**: ~5-8 minutes (critical path only)
- **Optimized build**: ~8-12 minutes (best balance)

Cache hits can reduce build times by 50-70%.

## Maintenance

- Workflows use latest stable actions versions
- MSBuild and Visual Studio versions are pinned to windows-2022 runner
- Cache keys include file hashes for intelligent invalidation
- Retention periods are configured for cost optimization