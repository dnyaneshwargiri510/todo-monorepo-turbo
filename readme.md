npm install -g corepack@latest
pnpm init
echo "packages:\n  - 'packages/*'" > pnpm-workspace.yaml
pnpm add -D turbo

cat << 'EOF' > turbo.json
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**", ".next/**"]
    }
  }
}
EOF

pnpm pkg set scripts.build="turbo run build"

git remote set-url origin git@github-transperfect:dnyaneshwargiri510/todo-monorepo-turbo.git

pnpm --filter @todo/web dev

pnpm turbo run build --graph
pnpm turbo run build --graph=d.svg

pnpm turbo prune @todo/web