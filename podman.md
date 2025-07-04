# Podman Development Guide: Issues, Solutions, and Best Practices

## Overview
This comprehensive guide covers the most common issues developers encounter when using Podman for development, particularly when transitioning from Docker Compose setups. Podman offers excellent Docker compatibility but has unique characteristics that require understanding for smooth development workflows.

## Major Issue Categories

### 1. Docker Compose Compatibility Issues

#### Feature Completeness Gaps
- **Podman Compose Limitations**: Podman Compose implements only a subset of Docker Compose functionality - works well for basic service definitions but lacks advanced features
- **Advanced Features**: Complex networking, volume plugins, and sophisticated orchestration features may require manual configuration or systemd integration
- **Version Compatibility**: Different behavior between Docker Compose v1 and v2 when used with Podman

#### Solutions
- Use `podman compose` (recommended) or `docker-compose` directly with Podman socket
- Enable Docker compatibility mode in Podman Desktop
- For complex scenarios, consider migrating to `podman generate kube` and Kubernetes YAML
- Test compose files thoroughly - simple projects work well, complex ones need validation

### 2. Networking Issues and Port Conflicts

#### Rootless vs Rootful Networking Differences
- **Rootless Mode**: Uses slirp4netns, more restrictive, requires careful port mapping
- **Rootful Mode**: Uses containernetworking plugins, more Docker-like behavior
- **Port Mapping Problems**: Random port assignment, UDP port mapping issues, privileged port restrictions

#### Common Networking Issues
```bash
# Issue: Port conflicts with system services
podman run -p 53:53/udp image  # Conflicts with dnsmasq

# Issue: External access problems in rootless mode
podman run -p 8080:80 nginx  # May not be accessible externally

# Issue: Container-to-container communication
# Containers not in pods can't communicate via localhost
```

#### Solutions
```bash
# Use host networking for development (insecure for production)
podman run --network host image

# Create pods for container communication
podman pod create --name myapp -p 8080:80
podman run --pod myapp --name web nginx
podman run --pod myapp --name db mysql

# Use --userns=keep-id for better port mapping
podman run --userns=keep-id -p 8080:80 image

# Switch network backend if needed
podman system reset --force
echo -e "[containers]\nnetwork_backend = \"cni\"" > ~/.config/containers/containers.conf
```

### 3. Volume Mapping and User ID Preservation Issues

#### User Namespace Mapping Problems
- **Root Mapping**: Host UID 1000 maps to container UID 0, causing permission issues
- **File Ownership**: Created files appear with unexpected UIDs on the host
- **Non-root Containers**: Processes running as non-root can't write to mounted volumes

#### Critical Issues
```bash
# Issue: Files created by container appear as different UID on host
podman run -v /host/path:/container/path image
# Files created may show as UID 102002 instead of 1000

# Issue: Permission denied for non-root container processes
podman run -v /host/path:/container/path --user 1000:1000 image
# Process can't write to mounted directory
```

#### Solutions
```bash
# Use :U suffix to fix ownership recursively
podman run -v /host/path:/container/path:U image

# Use --userns=keep-id to preserve user mapping
podman run --userns=keep-id -v /host/path:/container/path image

# Manual permission fixing with podman unshare
podman unshare chown 1000:1000 -R /host/path

# For rootful containers, use idmapped mounts
podman run --volume /host/path:/container/path:idmap image
```

### 4. Environment Variables and Script Execution Differences

#### Key Differences from Docker
- **Variable Expansion**: Podman handles environment variable expansion differently
- **Script Execution**: Subtle differences in how scripts are processed
- **Host Environment**: Podman's wildcard feature for environment variables

#### Issues and Solutions
```bash
# Issue: Environment variables not expanded in CMD
# Use proper shell form in Containerfile
CMD ["sh", "-c", "echo $MY_VAR"]

# Issue: Scripts with Docker-specific assumptions
# Ensure scripts are compatible with Podman's rootless mode

# Use Podman's wildcard feature for environment variables
podman run -e "APP_*" image  # Imports all APP_* variables from host
```

### 5. SELinux Security Context Issues

#### Volume Access Problems
- **Default Context**: Containers can't access volumes without proper SELinux labels
- **Shared vs Private**: Confusion between :z and :Z options
- **Symlink Issues**: SELinux relabeling problems with symlinks

#### Solutions
```bash
# Use :Z for private volumes (recommended)
podman run -v /host/path:/container/path:Z image

# Use :z for shared volumes (security risk)
podman run -v /host/path:/container/path:z image

# Check SELinux context
ls -lZ /host/path
restorecon -Rv /host/path
```

## Best Practices for Development

### 1. Development Environment Setup
```bash
# Configure Podman for development
echo -e "[containers]\nnetwork_backend = \"cni\"\nuserns = \"keep-id\"" > ~/.config/containers/containers.conf

# Create development pod template
podman pod create --name dev-stack -p 3000:3000 -p 8080:8080 -p 5432:5432

# Use rootless mode for security
podman run --userns=keep-id -v ./src:/app:U node:18
```

### 2. Container Naming and Management
```bash
# Use consistent naming convention
podman run --name myapp-web-dev image

# Avoid port conflicts with systematic port allocation
# Dev: 3000-3099, Test: 3100-3199, etc.
podman run -p 3001:3000 --name myapp-web-dev image
```

### 3. Volume Management
```bash
# Create named volumes for persistent data
podman volume create myapp-data
podman run -v myapp-data:/data:Z image

# Use bind mounts with proper flags for development
podman run -v $(pwd):/app:Z,U --workdir /app node:18
```

### 4. Systemd Integration for Services
```bash
# Generate systemd service files using Quadlet
mkdir -p ~/.config/containers/systemd
cat > ~/.config/containers/systemd/myapp.container << EOF
[Unit]
Description=My App Container
After=network.target

[Container]
Image=myapp:latest
PublishPort=8080:8080
Volume=/home/user/data:/data:Z

[Service]
Restart=always

[Install]
WantedBy=multi-user.target
EOF

# Reload and enable
systemctl --user daemon-reload
systemctl --user enable myapp.service
systemctl --user start myapp.service
```

## Production Considerations

### 1. Security
- Always use rootless mode in production
- Implement proper SELinux labeling
- Use secrets management instead of environment variables
- Enable read-only root filesystem where possible

### 2. Performance
- Use volumes instead of bind mounts for better performance
- Configure appropriate restart policies
- Monitor resource usage with `podman stats`
- Use multi-stage builds to reduce image size

### 3. Monitoring and Logging
```bash
# View container logs
podman logs -f container-name

# Monitor resource usage
podman stats container-name

# Health checks
podman healthcheck run container-name
```

## Migration Strategies

### From Docker Compose to Podman
1. **Test Compose Files**: Start with `podman-compose` for basic validation
2. **Identify Issues**: Run through this checklist for compatibility problems
3. **Gradual Migration**: Move services one at a time
4. **Consider Alternatives**: Evaluate Kubernetes YAML with `podman play kube`

### From Docker to Podman
1. **Alias Docker Commands**: `alias docker=podman`
2. **Update Build Scripts**: Replace Docker-specific flags
3. **Test Thoroughly**: Validate all workflows in development
4. **Update Documentation**: Document Podman-specific configurations

## Troubleshooting Commands

### Diagnostic Commands
```bash
# Check Podman configuration
podman info

# Debug networking issues
podman network ls
podman network inspect bridge

# Check user namespace mapping
podman unshare cat /proc/self/uid_map

# Verify SELinux contexts
podman run --rm -v /host/path:/test:Z alpine ls -lZ /test

# Check systemd integration
systemctl --user status container-name
```

### Common Fixes
```bash
# Reset Podman configuration
podman system reset

# Fix permission issues
podman unshare chown -R $(id -u):$(id -g) /path/to/volume

# Clear cached images and containers
podman system prune -a

# Restart user services
systemctl --user daemon-reload
```

## Performance Optimization

### Container Optimization
- Use minimal base images (Alpine, distroless)
- Implement proper layer caching in Containerfiles
- Use multi-stage builds for smaller production images
- Enable BuildKit equivalent features in Podman

### Resource Management
```bash
# Set resource limits
podman run --memory=512m --cpus=1.0 image

# Use cgroups v2 for better resource management
podman run --cgroup-manager=systemd image
```

## Integration with Development Tools

### IDE Integration
- VS Code: Use Podman extension for container management
- IntelliJ: Configure Docker plugin to use Podman socket
- CLI tools: Update scripts to use `podman` instead of `docker`

### CI/CD Integration
```yaml
# GitLab CI example
build:
  script:
    - podman build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - podman push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
```

## Final Recommendations

1. **Start Simple**: Begin with basic containers before complex compose setups
2. **Test Thoroughly**: Always validate your specific use cases
3. **Document Changes**: Keep track of Podman-specific configurations
4. **Monitor Performance**: Watch for resource usage differences
5. **Stay Updated**: Follow Podman releases for compatibility improvements
6. **Use Rootless**: Embrace rootless containers for better security
7. **Leverage Systemd**: Use systemd integration for production services

This guide should help you navigate the most common pitfalls and successfully implement Podman in your development workflow.

---

## Project-Level CLAUDE.md Instructions

Add these concise reminders to your project's CLAUDE.md file:

```markdown
# Podman Development Reminders

## Essential Flags for Development
- Use `--userns=keep-id` for proper user mapping in rootless mode
- Use `:Z` suffix for private volume mounts, `:U` for ownership fixes
- Use `podman compose` instead of `docker-compose` for better compatibility
- Always test rootless mode first, use rootful only when necessary

## Common Issues & Quick Fixes
- Port conflicts: Use `--network host` for development, pods for multi-container
- Permission denied: Add `:Z` to volume mounts, use `podman unshare chown` if needed
- Container communication: Create pods with `podman pod create` for shared networking
- SELinux issues: Use `:Z` for private volumes, check contexts with `ls -lZ`

## Best Practices
- Use systematic port allocation (dev: 3000-3099, test: 3100-3199, etc.)
- Create named volumes for persistent data instead of bind mounts
- Test both `podman compose` and `podman play kube` workflows
- Use `podman info` and `podman network ls` for troubleshooting

## Production Notes
- Never use `--network host` in production
- Implement proper resource limits with `--memory` and `--cpus`
- Use systemd integration with Quadlet for service management
- Always validate Docker Compose compatibility before deployment
```