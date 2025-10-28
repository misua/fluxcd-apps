# FluxCD Applications Repository

This repository contains application manifests for multi-environment GitOps deployment using FluxCD.

## Repository Structure

```
fluxcd-apps/
├── apps/              # Application manifests
│   ├── base/          # Base application resources
│   └── overlays/      # Environment-specific overlays
│       ├── dev/
│       ├── staging/
│       └── production/
└── clusters/          # GitRepository and Kustomization resources
    ├── dev/
    ├── staging/
    └── production/
```

## Environments

- **dev**: Automated sync from `main` branch (1-minute interval)
- **staging**: Manual promotion via Git tags (e.g., `staging-v1.0.0`)
- **production**: Manual promotion via Git tags (e.g., `prod-v1.0.0`)

## Application Deployment

Applications are deployed using Kustomize overlays with environment-specific configurations:
- Replica counts
- Resource limits/requests
- Environment variables
- ConfigMaps and Secrets
- Ingress rules

## Adding a New Application

1. Create base manifests in `apps/base/<app-name>/`
2. Create environment overlays in `apps/overlays/{dev,staging,production}/<app-name>/`
3. Update the kustomization.yaml in each overlay to include the new app
4. Commit and push to trigger deployment (dev) or create a tag (staging/prod)

## Promotion Workflow

### Staging Promotion
```bash
git tag -a staging-v1.0.0 -m "Promote to staging"
git push origin staging-v1.0.0
```

### Production Promotion
```bash
git tag -a prod-v1.0.0 -m "Promote to production"
git push origin prod-v1.0.0
```

## Monitoring

Check application sync status:
```bash
flux get kustomizations
flux get helmreleases
```

## Related Repositories

- [fluxcd-infrastructure](https://github.com/misua/fluxcd-infrastructure) - Infrastructure manifests
