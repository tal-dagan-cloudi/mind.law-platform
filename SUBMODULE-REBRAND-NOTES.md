# Submodule Rebranding Notes

## Status: Forked and Linked ✅

The following submodules have been forked to your GitHub account and linked to this repository:

1. **Browser Extension**
   - Original: https://github.com/Mintplex-Labs/anythingllm-extension.git
   - Fork: https://github.com/tal-dagan-cloudi/mindlaw-extension.git
   - Status: ✅ Forked and linked in `.gitmodules`

2. **Embed Widget**
   - Original: https://github.com/Mintplex-Labs/anythingllm-embed.git
   - Fork: https://github.com/tal-dagan-cloudi/mindlaw-embed.git
   - Status: ✅ Forked and linked in `.gitmodules`

## Next Steps: Internal Rebranding

The submodules themselves still contain AnythingLLM branding internally. To complete the rebrand:

### Browser Extension (`browser-extension/`)
1. Navigate to submodule: `cd browser-extension`
2. Create feature branch: `git checkout -b rebrand-to-mindlaw`
3. Update all references:
   ```bash
   find . -type f \( -name "*.js" -o -name "*.json" -o -name "*.md" -o -name "*.html" \) -exec sed -i '' -e 's/AnythingLLM/Mind.Law/g' -e 's/anythingllm/mindlaw/g' -e 's/anything-llm/mind-law/g' {} +
   ```
4. Update manifest.json with new extension name
5. Commit: `git commit -am "Rebrand to Mind.Law"`
6. Push: `git push origin rebrand-to-mindlaw`
7. Create PR on GitHub and merge
8. Return to parent repo and update submodule reference

### Embed Widget (`embed/`)
1. Navigate to submodule: `cd embed`
2. Create feature branch: `git checkout -b rebrand-to-mindlaw`
3. Update all references:
   ```bash
   find . -type f \( -name "*.js" -o -name "*.jsx" -o -name "*.json" -o -name "*.md" -o -name "*.html" \) -exec sed -i '' -e 's/AnythingLLM/Mind.Law/g' -e 's/anythingllm/mindlaw/g' -e 's/anything-llm/mind-law/g' {} +
   ```
4. Update package.json and README
5. Commit: `git commit -am "Rebrand to Mind.Law"`
6. Push: `git push origin rebrand-to-mindlaw`
7. Create PR on GitHub and merge
8. Return to parent repo and update submodule reference

## Important Notes

- The submodules are currently pointing to the default branches of your forks
- The forks still contain the original AnythingLLM branding
- You can rebrand them at any time by following the steps above
- Once rebranded, update the parent repository's submodule references to point to the rebranded commits

## Alternative: Remove Submodules

If you don't need the browser extension or embed widget features, you can remove the submodules:

```bash
git submodule deinit -f browser-extension
git rm -f browser-extension
git submodule deinit -f embed
git rm -f embed
git commit -m "Remove unused submodules"
```
