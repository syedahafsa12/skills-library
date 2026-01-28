# AGENTS.md
Repository structure and guidelines for AI agents

## Root Files
- `.claude\settings.local.json`
- `.claude\skills\agents-md-gen\REFERENCE.md`
- `.claude\skills\agents-md-gen\SKILL.md`
- `.claude\skills\agents-md-gen\scripts\generate_agents_md.py`
- `.claude\skills\docusaurus-deploy\REFERENCE.md`
- `.claude\skills\docusaurus-deploy\SKILL.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docusaurus.config.ts`
- `.claude\skills\docusaurus-deploy\learnflow-docs\sidebars.ts`
- `.claude\skills\docusaurus-deploy\learnflow-docs\tsconfig.json`
- `.claude\skills\docusaurus-deploy\learnflow-docs\blog\2019-05-28-first-blog-post.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\blog\2019-05-29-long-blog-post.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\blog\2021-08-01-mdx-blog-post.mdx`
- `.claude\skills\docusaurus-deploy\learnflow-docs\blog\authors.yml`
- `.claude\skills\docusaurus-deploy\learnflow-docs\blog\tags.yml`
- `.claude\skills\docusaurus-deploy\learnflow-docs\blog\2021-08-26-welcome\docusaurus-plushie-banner.jpeg`
- `.claude\skills\docusaurus-deploy\learnflow-docs\blog\2021-08-26-welcome\index.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\intro.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-basics\congratulations.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-basics\create-a-blog-post.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-basics\create-a-document.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-basics\create-a-page.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-basics\deploy-your-site.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-basics\markdown-features.mdx`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-basics\_category_.json`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-extras\manage-docs-versions.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-extras\translate-your-site.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-extras\_category_.json`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-extras\img\docsVersionDropdown.png`
- `.claude\skills\docusaurus-deploy\learnflow-docs\docs\tutorial-extras\img\localeDropdown.png`
- `.claude\skills\docusaurus-deploy\learnflow-docs\src\components\HomepageFeatures\index.tsx`
- `.claude\skills\docusaurus-deploy\learnflow-docs\src\components\HomepageFeatures\styles.module.css`
- `.claude\skills\docusaurus-deploy\learnflow-docs\src\css\custom.css`
- `.claude\skills\docusaurus-deploy\learnflow-docs\src\pages\index.module.css`
- `.claude\skills\docusaurus-deploy\learnflow-docs\src\pages\index.tsx`
- `.claude\skills\docusaurus-deploy\learnflow-docs\src\pages\markdown-page.md`
- `.claude\skills\docusaurus-deploy\learnflow-docs\static\.nojekyll`
- `.claude\skills\docusaurus-deploy\learnflow-docs\static\img\docusaurus-social-card.jpg`
- `.claude\skills\docusaurus-deploy\learnflow-docs\static\img\docusaurus.png`
- `.claude\skills\docusaurus-deploy\learnflow-docs\static\img\favicon.ico`
- `.claude\skills\docusaurus-deploy\learnflow-docs\static\img\logo.svg`
- `.claude\skills\docusaurus-deploy\learnflow-docs\static\img\undraw_docusaurus_mountain.svg`
- `.claude\skills\docusaurus-deploy\learnflow-docs\static\img\undraw_docusaurus_react.svg`
- `.claude\skills\docusaurus-deploy\learnflow-docs\static\img\undraw_docusaurus_tree.svg`
- `.claude\skills\docusaurus-deploy\scripts\build_and_deploy.sh`
- `.claude\skills\docusaurus-deploy\scripts\configure_site.sh`
- `.claude\skills\docusaurus-deploy\scripts\deploy.sh`
- `.claude\skills\docusaurus-deploy\scripts\verify_deployment.py`
- `.claude\skills\docusaurus-deploy\templates\docusaurus.config.js`
- `.claude\skills\fastapi-dapr-agent\REFERENCE.md`
- `.claude\skills\fastapi-dapr-agent\SKILL.md`
- `.claude\skills\fastapi-dapr-agent\--type\triage-agent\main.py`
- `.claude\skills\fastapi-dapr-agent\scripts\add_dapr_config.sh`
- `.claude\skills\fastapi-dapr-agent\scripts\create_agent_service.py`
- `.claude\skills\fastapi-dapr-agent\scripts\create_service.py`
- `.claude\skills\fastapi-dapr-agent\scripts\create_service.sh`
- `.claude\skills\fastapi-dapr-agent\scripts\deploy_service.sh`
- `.claude\skills\fastapi-dapr-agent\scripts\deploy_to_k8s.sh`
- `.claude\skills\fastapi-dapr-agent\scripts\verify_deployment.py`
- `.claude\skills\fastapi-dapr-agent\scripts\verify_service.py`
- `.claude\skills\k8s-foundation\REFERENCE.md`
- `.claude\skills\k8s-foundation\SKILL.md`
- `.claude\skills\k8s-foundation\scripts\check_cluster_health.py`
- `.claude\skills\k8s-foundation\scripts\setup_foundation.sh`
- `.claude\skills\k8s-foundation\scripts\verify_readiness.py`
- `.claude\skills\kafka-k8s-setup\REFERENCE.md`
- `.claude\skills\kafka-k8s-setup\SKILL.md`
- `.claude\skills\kafka-k8s-setup\scripts\create_topics.py`
- `.claude\skills\kafka-k8s-setup\scripts\create_topics.sh`
- `.claude\skills\kafka-k8s-setup\scripts\deploy.sh`
- `.claude\skills\kafka-k8s-setup\scripts\deploy_kafka.sh`
- `.claude\skills\kafka-k8s-setup\scripts\deploy_simple.sh`
- `.claude\skills\kafka-k8s-setup\scripts\verify.py`
- `.claude\skills\kafka-k8s-setup\scripts\verify_kafka.py`
- `.claude\skills\mcp-code-execution\REFERENCE.md`
- `.claude\skills\mcp-code-execution\SKILL.md`
- `.claude\skills\mcp-code-execution\scripts\integration_demo.py`
- `.claude\skills\mcp-code-execution\scripts\mcp_client.py`
- `.claude\skills\mcp-code-execution\scripts\run_examples.sh`
- `.claude\skills\mcp-code-execution\scripts\examples\k8s_health_check.py`
- `.claude\skills\mcp-code-execution\scripts\examples\kafka_monitor.py`
- `.claude\skills\mcp-code-execution\scripts\examples\postgres_health_check.py`
- `.claude\skills\nextjs-k8s-deploy\REFERENCE.md`
- `.claude\skills\nextjs-k8s-deploy\SKILL.md`
- `.claude\skills\nextjs-k8s-deploy\scripts\build_deploy.sh`
- `.claude\skills\nextjs-k8s-deploy\scripts\configure_env.sh`
- `.claude\skills\nextjs-k8s-deploy\scripts\verify_deployment.py`
- `.claude\skills\nextjs-k8s-deploy\scripts\verify_nextjs.py`
- `.claude\skills\nextjs-k8s-deploy\templates\Dockerfile.nextjs`
- `.claude\skills\nextjs-k8s-deploy\templates\k8s\deployment.yaml`
- `.claude\skills\postgres-k8s-setup\REFERENCE.md`
- `.claude\skills\postgres-k8s-setup\SKILL.md`
- `.claude\skills\postgres-k8s-setup\scripts\apply_schema.sh`
- `.claude\skills\postgres-k8s-setup\scripts\deploy.sh`
- `.claude\skills\postgres-k8s-setup\scripts\deploy_postgres.sh`
- `.claude\skills\postgres-k8s-setup\scripts\run_migrations.py`
- `.claude\skills\postgres-k8s-setup\scripts\run_migrations.sh`
- `.claude\skills\postgres-k8s-setup\scripts\verify.py`
- `.claude\skills\postgres-k8s-setup\scripts\verify_postgres.py`
- `docs\skill-development-guide.md`

## Configuration Files
- `README.md` - Important configuration file
- `.claude\skills\docusaurus-deploy\learnflow-docs\package.json` - Important configuration file
- `.claude\skills\docusaurus-deploy\learnflow-docs\README.md` - Important configuration file
- `.claude\skills\fastapi-dapr-agent\--type\triage-agent\Dockerfile` - Important configuration file
- `.claude\skills\fastapi-dapr-agent\--type\triage-agent\requirements.txt` - Important configuration file

## Directory Structure
## File Extensions and Technologies
Common file extensions and their purposes in this repository:
- `.py`: Python source code
- `.js`, `.jsx`: JavaScript/JSX code
- `.ts`, `.tsx`: TypeScript/TSX code
- `.md`: Documentation files
- `.yaml`, `.yml`: Configuration files
- `.json`: Data and configuration files
- `.sh`: Shell scripts
- `.sql`: Database scripts

## Guidelines for AI Agents
1. Respect the existing code style and patterns
2. Follow the established architecture and design principles
3. Maintain backward compatibility when possible
4. Update documentation when making changes
5. Consider the impact on related components
