# Next Steps

## Overview

The transformation appears to be successful with no build errors reported across any of the projects in the solution. All five projects (Bookstore.Data, Bookstore.Domain.Tests, Bookstore.Cdk, Bookstore.Web, and Bookstore.Domain) have compiled without issues.

## Validation Steps

### 1. Verify Target Framework

Confirm that all projects are targeting the appropriate .NET version:

```bash
dotnet list package --framework
```

Review each `.csproj` file to ensure the `<TargetFramework>` element specifies a cross-platform .NET version (e.g., `net6.0`, `net7.0`, or `net8.0`).

### 2. Run Unit Tests

Execute the test project to ensure existing functionality remains intact:

```bash
cd app/Bookstore.Domain.Tests
dotnet test --verbosity normal
```

Review test results for any failures or warnings that may indicate compatibility issues.

### 3. Verify Dependencies

Check for any deprecated or platform-specific NuGet packages:

```bash
dotnet list package --deprecated
dotnet list package --vulnerable
```

Update any flagged packages to their latest stable versions compatible with cross-platform .NET.

### 4. Test Database Connectivity

Since the solution includes a Data project, verify database connections work correctly:

- Test connection strings in configuration files
- Run the application and perform basic CRUD operations
- Verify Entity Framework migrations (if applicable) function correctly:

```bash
cd app/Bookstore.Data
dotnet ef migrations list
```

### 5. Run the Web Application Locally

Start the web application and verify it functions as expected:

```bash
cd app/Bookstore.Web
dotnet run
```

Test the following:
- Application starts without errors
- All endpoints respond correctly
- Static files are served properly
- Authentication/authorization works (if applicable)

### 6. Review Configuration Files

Examine configuration files for platform-specific paths or settings:

- Check `appsettings.json` and `appsettings.Development.json`
- Verify file paths use forward slashes or `Path.Combine()`
- Ensure connection strings are appropriate for the target environment

### 7. Test on Target Platforms

Deploy and test the application on the intended operating systems:

- **Linux**: Test on a Linux distribution (Ubuntu, Debian, etc.)
- **macOS**: Test on macOS if applicable
- **Windows**: Verify continued compatibility with Windows

For each platform:

```bash
dotnet publish -c Release -r <runtime-identifier>
```

Common runtime identifiers: `linux-x64`, `osx-x64`, `win-x64`

### 8. Validate CDK Project

Since the solution includes a CDK project, verify AWS infrastructure code:

```bash
cd app/Bookstore.Cdk
dotnet build
cdk synth
```

Review the synthesized CloudFormation template for any issues.

### 9. Performance Testing

Compare application performance before and after migration:

- Measure startup time
- Test response times for key endpoints
- Monitor memory usage
- Check for any performance regressions

### 10. Code Review

Perform a manual code review focusing on:

- Removal of Windows-specific APIs (e.g., Registry access, Windows-only P/Invoke calls)
- File path handling (ensure cross-platform compatibility)
- Line ending handling in file operations
- Case sensitivity in file and directory names

## Deployment Preparation

### 1. Update Documentation

- Update README files with new build and run instructions
- Document the target .NET version
- Update deployment guides for cross-platform scenarios

### 2. Environment Configuration

- Set up environment variables for different deployment targets
- Configure platform-specific settings in hosting environments
- Update any deployment scripts to use `dotnet` CLI commands

### 3. Prepare Deployment Package

Create a self-contained deployment if needed:

```bash
dotnet publish -c Release -r <runtime-identifier> --self-contained true
```

Or framework-dependent deployment:

```bash
dotnet publish -c Release
```

### 4. Deploy to Target Environment

- Deploy the application to your staging environment first
- Perform smoke tests to verify basic functionality
- Monitor application logs for any runtime errors
- Validate all integrations (databases, external APIs, etc.)

### 5. Production Deployment

Once staging validation is complete:

- Deploy to production using your standard deployment process
- Monitor application health and performance metrics
- Keep the previous version available for quick rollback if needed
- Verify all production features function correctly

## Post-Deployment Monitoring

- Monitor application logs for unexpected errors
- Track performance metrics and compare with baseline
- Collect user feedback on any functional changes
- Document any platform-specific issues encountered

## Additional Considerations

- Ensure your hosting environment has the appropriate .NET runtime installed
- Update any build servers or CI/CD agents with the new .NET SDK
- Review and update any automation scripts that reference the old framework
- Consider enabling ReadyToRun compilation for improved startup performance