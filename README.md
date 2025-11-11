# Posts Repository - Documentation Overview

This repository contains technical documentation and guides for various development topics. Below is a summary of each file's content:

## Files in this Repository

### 1. MigrationStep.md
**Topic:** Migration Step – Renaming properties and content types

This document explains how to handle renaming of properties and content types in Optimizely CMS without losing data:
- Covers when GUIDs protect against data loss during content type renaming
- Explains the importance of migration steps when renaming properties
- Provides code examples for renaming properties and content types using the `MigrationStep` class
- Key takeaway: Use `UsedToBeNamed()` method to map old names to new ones

### 2. NodeVersionIssue.md
**Topic:** Fixing Node.js version issues

A troubleshooting guide for resolving Node.js version compatibility errors during npm builds:
- Addresses the common `error:0308010C: digital envelope routines::unsupported` error
- Provides step-by-step instructions for using nvm (Node Version Manager)
- Includes commands for checking versions, installing specific Node versions, and switching between them
- Recommends cleaning up and reinstalling dependencies after version changes

### 3. SwitchGitUserPrFolder.md
**Topic:** Switching git user per folder on macOS

Explains how to configure different git identities for different project folders:
- Solves the problem of having separate personal and professional git configurations
- Shows how to set local git user email for specific project folders
- Provides commands for checking and setting local vs. global git configuration
- Useful for developers working on both personal and client projects

### 4. VirtualAndOverride.md
**Topic:** Use of virtual and override in C# methods

Demonstrates the practical application of virtual and override keywords in C#:
- Explains how virtual methods in base classes allow derived classes to extend behavior
- Provides an example using Excel worksheet header creation
- Shows how overriding improves code organization and maintainability
- Compares code before and after using virtual/override pattern

## Purpose

This repository serves as a collection of practical development tips and solutions to common problems encountered in software development, particularly focused on .NET/C#, Optimizely CMS, Node.js, and git configuration.
