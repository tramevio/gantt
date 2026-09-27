# Tramevio Gantt

**Interactive Gantt charts for project and resource planning.**

Tramevio is a framework-independent JavaScript library with TypeScript declarations and no runtime dependencies. Add project timelines, resource schedules and dependency views to your application while keeping control of your data and business rules.

This is the public distribution and issue-tracking repository for **`@tramevio/gantt`**. Development sources are maintained separately. The library is distributed under a [proprietary licence](LICENSE); a public repository does not make it open source.

## Features

- **Project planning:** tasks, groups, milestones, progress, baselines and task dependencies.
- **Resource planning:** resource schedules and interactive assignment editing.
- **Interactive timelines:** zoom, pan, date scales, search, filters and collapsible task hierarchies.
- **Configurable grid:** custom columns, sorting, resizing, reordering and editable cells.
- **Controlled editing:** the view proposes changes; your application validates and applies them. Transaction history supports undo and redo when enabled.
- **Date handling:** explicit date-only and timestamp modes, time zones and exclusive end dates.
- **Dependency analysis:** PERT-style network views, critical paths and scheduling margins.
- **Customisation:** themes, localisation, labels, tooltips and rendering extensions.
- **Exports:** SVG, browser-generated images, print layouts, JSON archives and CSV.
- **Integration:** a DOM-independent core, a browser entry point and a standalone script build.

## Release status

The current release series is **`0.1.0-beta`**. APIs may change before 1.0. Pin the version you evaluate and test it in your target environment before deployment.

This beta is not presented as production-certified. Browser coverage, accessibility and performance must be assessed against your application's requirements; mobile and touch use have not been validated.

## Installation

### GitHub Packages

The published package is available on [GitHub Packages](https://github.com/tramevio/gantt/pkgs/npm/gantt).

Add this scope mapping to your project's `.npmrc`:

```ini
@tramevio:registry=https://npm.pkg.github.com
```

Authenticate with your own GitHub account and a personal access token (classic) with `read:packages` permission, then install:

```sh
npm login --scope=@tramevio --auth-type=legacy --registry=https://npm.pkg.github.com
npm install @tramevio/gantt@0.1.0-beta.2
```

GitHub requires authentication to install npm packages from its registry, including public packages. Keep tokens out of committed files. See [GitHub's registry instructions](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry).

### npmjs.com

Publication to npmjs.com is being prepared. The GitHub package listing does not indicate availability on npmjs.com; the two registries are separate.

### Package contents

The distribution contains compiled JavaScript, TypeScript declarations and the licence. Development sources, tests and source maps are not included.

The package currently declares **Node.js 24.x** support for Node-based use and tooling. The browser component runs in the browser and requires a DOM container.

## Quick start

In an application using a JavaScript bundler, add a container:

```html
<div id="gantt"></div>
```

Then mount a project:

```js
import { mountGantt } from '@tramevio/gantt/browser';

const project = {
  schemaVersion: 1,
  kind: 'project',
  id: 'first-project',
  name: 'Product launch',
  time: { mode: 'date' },
  tasks: [
    {
      id: 'design', parentId: null, kind: 'task', name: 'Design',
      schedule: { start: '2026-10-01', end: '2026-10-04' },
      progress: 1,
    },
    {
      id: 'build', parentId: null, kind: 'task', name: 'Build',
      schedule: { start: '2026-10-04', end: '2026-10-09' },
      progress: 0.25,
    },
  ],
  dependencies: [
    { id: 'design-build', sourceId: 'design', targetId: 'build', type: 'FS' },
  ],
};

const container = document.getElementById('gantt');
if (!container) throw new Error('Gantt container not found');

const result = mountGantt(container, project, {
  start: '2026-10-01',
  end: '2026-10-12',
  width: 960,
});

if (!result.ok) {
  console.error('Unable to mount the project', result);
} else {
  const view = result.view;
  // Keep this reference and call view.destroy() when your component unmounts.
}
```

Task intervals use **`[start, end)`**: the start is included and the end is excluded. In this example, Design occupies October 1, 2 and 3.

This example displays and navigates a project. Editing is opt-in: enabling it requires your application to handle proposed changes and supply the updated document.

### Entry points

| Entry point | Purpose |
| --- | --- |
| `@tramevio/gantt` | DOM-independent data, validation, transactions and rendering APIs |
| `@tramevio/gantt/browser` | Browser views and browser export utilities |
| `dist/tramevio.global.min.js` | Standalone script exposing the `Tramevio` global |

The standalone build is included in the installed package. Serve it from your application's assets if you prefer script tags to a bundler. Framework-specific adapters are not required to mount the browser view; manage its lifecycle from your component.

## Feedback and support

Report reproducible defects and request features through [GitHub Issues](https://github.com/tramevio/gantt/issues). Include the package version, browser or Node.js version, a minimal example, and the expected and actual behaviour. Remove private project data and credentials before posting.

This repository hosts public distribution information and feedback, not the development source tree. Code contributions require a separate contributor agreement. No support response time or service-level commitment is included with the free licence.

## Licence

Tramevio uses the [Tramevio Software Licence](LICENSE), a proprietary licence.

- Qualifying non-commercial use is permitted under its conditions.
- Evaluation for a possible commercial purchase is permitted outside production.
- Professional and other uses outside the free grant require a written commercial licence, including paid products, SaaS and internal business applications.
- Standalone redistribution, modifications and competing library products are restricted as described in the licence.

For commercial licensing, contact [tramavio@gmail.com](mailto:tramavio@gmail.com).

The licence shipped with each package governs that version unless a separate written agreement applies. This overview does not replace the full licence.
