# PoC Plan: LazyCodex

## Project Classification
- **Type:** web-app
- **Key Technologies:** Next.js 16, React 19, TypeScript, pnpm, Node.js 22
- **ODH Relevance:** Demonstrates containerized deployment of a modern JavaScript web application on OpenShift AI

## PoC Objectives
1. Validate that a Next.js 16 application can be containerized with UBI-based Node.js images
2. Verify the web application builds and serves correctly in an OpenShift environment
3. Confirm the application responds to HTTP requests on health and content endpoints

## Infrastructure Requirements
- **Resource Profile:** small (256Mi RAM, 250m CPU)
- **GPU Required:** no
- **Persistent Storage:** none
- **Sidecar Containers:** none
- **Deployment Model:** deployment
- **Listens on Port:** true (port 3000)
- **Test Strategy:** http

## Test Scenarios

### Scenario 1: health-check
- **Description:** Verify the Next.js application responds to root path
- **Type:** http
- **Input:** GET /
- **Expected:** Returns 200 OK with HTML content
- **Timeout:** 60 seconds

### Scenario 2: static-assets
- **Description:** Verify static assets are served correctly
- **Type:** http
- **Input:** GET /_next/static (any static asset request)
- **Expected:** Returns 200 or redirect
- **Timeout:** 30 seconds

## Dockerfile Considerations
- Use `registry.access.redhat.com/ubi9/nodejs-22` as base image
- Install pnpm globally (required by the project)
- Build the Next.js app with `pnpm build`
- Serve with `node .next/standalone/server.js` (Next.js standalone output)
- Enable `output: "standalone"` in next.config.ts if not already set

## Deployment Considerations
- Deploy as a Kubernetes Deployment with a Service on port 3000
- Readiness probe on GET /
- Single replica is sufficient for PoC
