# PRAXO-CORE PHP Core

PRAXO-CORE is an embeddable, MIT-licensed agent framework. This package is its
native PHP Core: an agent loop with tool registration and dispatch, policy
hooks, typed events, memory, checkpoints, context budgets, middleware, JSON
Schema validation, and plugin manifest parsing. It runs in-process and does not
require a hosted service.

- `SPEC_VERSION`: `0.2.0`
- Core version: `0.4.0`
- License: MIT
- Dependencies: none beyond the PHP standard library

This repository is the PHP publication split of
[`axisvela/praxo-core`](https://github.com/axisvela/praxo-core). The source of
truth and conformance vectors remain in the main repository.

## Install

```bash
composer require axisvela/praxo-core
```

## Scope

The Core implements all eleven vector groups: toolcall, jsonschema, plugin,
checkpoint, event, memory, policy, stream, context, middleware, agent loop, and
human-in-the-loop interrupt decisions. It includes a public API snapshot and
dependency-free tests. Provider adapters are not included in `0.4.0`.
Caller-provided `batchToolInvoker` and `maxParallelTools` enable cooperative
parallel tool dispatch without Core-created threads.

## Validation

This split repository contains the Composer package only. Conformance, API
snapshot, and dependency-free tests run in the
[main repository](https://github.com/axisvela/praxo-core/tree/main/language/php):

```bash
php language/php/conformance/run.php
php language/php/conformance/run.php --check-api
php language/php/tests/run.php
```

## Quickstart

```php
<?php

require __DIR__ . '/vendor/autoload.php';

use Praxo\Core\Agent;
use Praxo\Core\AgentOptions;
use Praxo\Core\Session;
use Praxo\Core\StaticModel;

$model = new StaticModel('hello');
$agent = new Agent(new AgentOptions($model));
$result = $agent->run(new Session('demo'), 'hello');
echo $result->output;
```

Tools use the same options object:

```php
use Praxo\Core\FuncTool;
use Praxo\Core\ToolRegistry;

$registry = new ToolRegistry();
$registry->register(new FuncTool('echo', 'Echo arguments', static fn (array $args): array => $args));

$options = new AgentOptions($model);
$options->tools = $registry;
$options->maxSteps = 8;
$agent = new Agent($options);
```

## Layout

- `src/` library (PSR-4 `Praxo\Core\`).
- `api/0.4.0.txt` checked-in public API snapshot.

## Release

Tag shape: `v0.4.0`. The tag targets `SPEC_VERSION` `0.2.0`.
