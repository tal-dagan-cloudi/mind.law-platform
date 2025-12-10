# Mind.Law Post-Rebrand Checklist

## ✅ Completed Rebrand Tasks

### Phase 1: Core Configuration ✅
- [x] Root package.json updated to `mind-law-platform`
- [x] Frontend package.json updated to `mindlaw-frontend`
- [x] Backend package.json updated to `mindlaw-server`
- [x] Collector package.json updated to `mindlaw-document-collector`
- [x] Database file reference updated to `mindlaw.db`
- [x] PWA manifest.json updated
- [x] index.html meta tags updated with mind.law domain

### Phase 2: Logo & Assets ✅
- [x] Logo placeholder instructions created
- [x] LogoContext.jsx updated to reference new filenames
- [x] Placeholder logo files created (copied from original)
- [ ] ⚠️ **ACTION REQUIRED**: Replace placeholder logos with actual Mind.Law brand assets

### Phase 3: Codebase-Wide Replacements ✅
- [x] 85 frontend files rebranded (557 replacements)
- [x] 65 backend/server files rebranded (246 replacements)
- [x] All code references updated across 150+ files

### Phase 4: Internationalization ✅
- [x] 25+ locale files updated with "Mind.Law" brand name
- [x] 4 locale documentation files (zh-CN, ja-JP, tr-TR, fa-IR) updated

### Phase 5: Docker & Containerization ✅
- [x] Dockerfile updated
- [x] docker-compose.yml service names updated
- [x] Docker environment files updated
- [x] HOW_TO_USE_DOCKER.md updated

### Phase 6: Cloud Deployments ✅
- [x] Helm chart directory renamed: `anythingllm` → `mindlaw`
- [x] All Helm templates and values updated (16 files)
- [x] AWS CloudFormation template renamed and updated
- [x] GCP deployment yaml renamed and updated
- [x] DigitalOcean Terraform configs updated
- [x] Kubernetes manifests updated
- [x] HuggingFace Spaces Dockerfile updated

### Phase 7: Documentation ✅
- [x] README.md comprehensively updated
- [x] CONTRIBUTING.md updated
- [x] BARE_METAL.md deployment guide updated
- [x] .devcontainer README updated
- [x] All technical documentation updated

### Phase 8: Embedded Widgets ✅
- [x] Embedded widget JS renamed: `mindlaw-chat-widget.min.js`
- [x] Embedded widget CSS renamed: `mindlaw-chat-widget.min.css`
- [x] Widget self-references updated

### Phase 9: Git Submodules ✅
- [x] Browser extension forked to: `tal-dagan-cloudi/mindlaw-extension`
- [x] Embed widget forked to: `tal-dagan-cloudi/mindlaw-embed`
- [x] .gitmodules updated to point to new forks
- [ ] ⚠️ **ACTION REQUIRED**: Rebrand content within submodules (see SUBMODULE-REBRAND-NOTES.md)

### Phase 10: GitHub Actions ✅
- [x] All workflow files updated with new Docker image names
- [x] Build and deployment workflows rebranded

### Phase 11: GitHub Repository ✅
- [x] New repository created: `tal-dagan-cloudi/mind.law-platform`
- [x] Git remote updated to point to new repository
- [ ] ⚠️ **NEXT**: Push to new repository (Phase 14)

### Phase 12: Swagger/API Documentation ✅
- [x] Updated as part of server code rebrand (Phase 3)

---

## 🔲 Remaining Tasks

### Logo Assets (CRITICAL)
You need to replace the placeholder logo files with actual Mind.Law branded assets:

**Required Files:**
1. `frontend/src/media/logo/mind-law.png` - Light theme logo
2. `frontend/src/media/logo/mind-law-dark.png` - Dark theme logo
3. `frontend/src/media/logo/mind-law-icon.png` - App icon
4. `frontend/src/media/logo/mind-law-infinity.png` - Compact logo
5. `frontend/public/favicon.png` - Browser favicon (32x32 or 64x64)
6. `frontend/public/favicon.ico` - Legacy icon (16x16, 32x32, 48x48)

See `/frontend/src/media/logo/README-LOGO-PLACEHOLDERS.md` for detailed specifications.

### Submodule Internal Rebranding (OPTIONAL)
The forked submodules still contain AnythingLLM branding internally:
- See `SUBMODULE-REBRAND-NOTES.md` for step-by-step instructions
- This can be done later when you need those features

### Build & Test (RECOMMENDED)
Before deploying, verify everything builds correctly:

```bash
# Install dependencies
npm run setup

# Build frontend
cd frontend && npm run build

# Build server (if applicable)
cd ../server && npm run build

# Run linting
cd .. && npm run lint

# Test Docker build
docker build -t mindlaw:latest -f docker/Dockerfile .
```

### Environment Configuration (REQUIRED FOR DEPLOYMENT)
Update environment variables for your deployment:
1. Copy `.env.example` files to `.env` in each directory
2. Configure database connections (update paths to `mindlaw.db`)
3. Set up API keys for LLM providers
4. Configure cloud provider credentials

### GitHub Repository Setup (RECOMMENDED)
After pushing:
1. Add repository description: "Mind.Law - AI-powered legal document management"
2. Add topics/tags: `legal-tech`, `ai`, `document-management`, `llm`
3. Set up branch protection rules on `master`
4. Configure GitHub Pages (if needed)
5. Set repository visibility (currently public)
6. Add LICENSE file review

### Deployment Checklist
- [ ] Update Helm values.yaml with your specific configurations
- [ ] Update CloudFormation parameters for AWS deployment
- [ ] Configure GCP project ID in deployment yaml
- [ ] Set up DigitalOcean API token for Terraform
- [ ] Configure Kubernetes secrets and config maps
- [ ] Update Docker Hub credentials for image publishing

---

## 📊 Rebrand Statistics

- **Total Files Modified**: 180+
- **Total Text Replacements**: 1000+
- **Package Files Updated**: 4
- **Locale Files Updated**: 25+
- **Docker/Cloud Configs**: 35+
- **Documentation Files**: 15+
- **Commits Made**: 11
- **New GitHub Repos Created**: 3 (main + 2 submodule forks)

---

## 🚀 Quick Start After Rebrand

```bash
# 1. Add Mind.Law logo assets
# (See frontend/src/media/logo/README-LOGO-PLACEHOLDERS.md)

# 2. Install dependencies
npm run setup

# 3. Start development environment
npm run dev:all

# 4. Build for production
npm run prod:frontend
npm run prod:server

# 5. Deploy with Docker
docker-compose up -d
```

---

## 🔍 Verification Commands

```bash
# Check for any remaining AnythingLLM references
grep -r "AnythingLLM" --exclude-dir=node_modules --exclude-dir=.git .
grep -r "anythingllm" --exclude-dir=node_modules --exclude-dir=.git .

# Verify package names
cat package.json | grep "name"
cat frontend/package.json | grep "name"
cat server/package.json | grep "name"
cat collector/package.json | grep "name"

# Verify Docker image names
cat docker/docker-compose.yml | grep image

# Verify Helm chart name
cat cloud-deployments/helm/charts/mindlaw/Chart.yaml | grep name
```

---

## 📞 Support

For issues or questions about the rebrand:
- Review commit history: `git log --oneline --graph --all`
- Check individual phase commits for changes made
- Refer to original AnythingLLM docs if needed (keeping in mind brand differences)
