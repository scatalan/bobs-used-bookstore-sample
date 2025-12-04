# Next Steps

## Overview

The transformation appears to be successful with no build errors reported across any of the projects in the solution. All five projects (Bookstore.Data, Bookstore.Domain.Tests, Bookstore.Cdk, Bookstore.Web, and Bookstore.Domain) have compiled without issues.

## Validation Steps

### 1. Verify Target Framework Configuration

Confirm that all projects are targeting the appropriate .NET version:

```bash
dotnet list package --framework
```

Review each `.csproj` file to ensure consistent target framework versions across the solution.

### 2. Run Unit Tests

Execute the test suite to verify functionality has been preserved:

```bash
cd app/Bookstore.Domain.Tests
dotnet test --verbosity normal
```

Review test results for any failures or warnings that may indicate runtime compatibility issues.

### 3. Check Package Dependencies

Verify all NuGet packages are compatible with the target framework:

```bash
dotnet list package --outdated
dotnet list package --deprecated
```

Update any outdated or deprecated packages to their cross-platform equivalents.

### 4. Validate Data Layer Functionality

Test database connectivity and data access operations:

- Verify connection strings are correctly configured for cross-platform environments
- Test Entity Framework migrations (if applicable)
- Confirm that database providers are compatible with the target .NET version

### 5. Test Web Application Locally

Run the web application to verify it functions correctly:

```bash
cd app/Bookstore.Web
dotnet run
```

Test the following:

- Application startup and configuration loading
- Routing and middleware pipeline
- Static file serving
- API endpoints (if applicable)
- Authentication and authorization flows

### 6. Review Configuration Files

Examine configuration files for platform-specific paths or settings:

- Check `appsettings.json` and environment-specific variants
- Verify file paths use cross-platform conventions (forward slashes or `Path.Combine`)
- Confirm environment variables are properly configured

### 7. Test on Target Platforms

Deploy and test the application on the intended target platforms:

- **Linux**: Test on a Linux distribution (Ubuntu, Debian, etc.)
- **macOS**: Verify functionality on macOS if applicable
- **Windows**: Confirm backward compatibility with Windows environments

### 8. Validate CDK Infrastructure Code

Review and test the CDK project:

```bash
cd app/Bookstore.Cdk
dotnet build
```

- Verify AWS CDK constructs are compatible with the .NET version
- Test infrastructure synthesis: `cdk synth`
- Validate stack definitions match requirements

### 9. Performance Testing

Conduct performance testing to identify any regressions:

- Compare application startup time with the legacy version
- Measure memory consumption under load
- Test response times for critical operations

### 10. Code Review for Platform-Specific Code

Search for potential platform-specific issues:

- Look for P/Invoke calls or native interop that may need adjustment
- Check for hardcoded Windows-specific paths (e.g., `C:\`, backslashes)
- Review any file I/O operations for cross-platform compatibility
- Verify registry access or Windows-specific APIs have been removed or abstracted

## Deployment Preparation

### 1. Create Publish Profiles

Generate publish profiles for target platforms:

```bash
# Self-contained deployment for Linux
dotnet publish -c Release -r linux-x64 --self-contained true

# Framework-dependent deployment
dotnet publish -c Release -r linux-x64 --self-contained false
```

### 2. Validate Published Output

Test the published application:

- Verify all required files are included in the publish output
- Check that configuration transforms are applied correctly
- Confirm dependencies are properly resolved

### 3. Update Documentation

Document the migration:

- Update README files with new build and run instructions
- Document any breaking changes or configuration updates
- Provide platform-specific setup instructions if needed

### 4. Establish Monitoring

Prepare for production monitoring:

- Verify logging frameworks are compatible and configured
- Test health check endpoints
- Confirm telemetry and diagnostics are functioning

## Final Checklist

- [ ] All projects build without errors or warnings
- [ ] Unit tests pass successfully
- [ ] Integration tests complete without failures
- [ ] Application runs correctly on target platforms
- [ ] Configuration files are platform-agnostic
- [ ] Dependencies are up-to-date and compatible
- [ ] Performance meets or exceeds legacy application
- [ ] Documentation has been updated
- [ ] Deployment artifacts have been validated