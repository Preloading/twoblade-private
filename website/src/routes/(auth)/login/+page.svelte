<script lang="ts">
    import { enhance } from '$app/forms';
    import type { ActionData } from './$types';
    import { Button } from '$lib/components/ui/button';
    import { Input } from '$lib/components/ui/input';
    import { Label } from '$lib/components/ui/label';
    import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '$lib/components/ui/card';
    import { Alert, AlertDescription, AlertTitle } from '$lib/components/ui/alert';
    import { AlertCircle } from 'lucide-svelte';

    let { form }: { form: ActionData } = $props();
    let username = $state('');
    let password = $state('');
    let isSubmitting = $state(false);

    function handleSubmit() {
        if (!username || !password) {
            return false;
        }
        isSubmitting = true;
    }

    $effect(() => {
        if (form) {
            isSubmitting = false;
            if (!form.success) {
                password = '';
            }
        }
    });
</script>

<div class="flex h-screen items-center justify-center">
    <Card class="w-[400px]">
        <CardHeader>
            <CardTitle>Login</CardTitle>
            <CardDescription>Welcome back! Please sign in to continue.</CardDescription>
        </CardHeader>
        <CardContent>
            <form
                method="POST"
                use:enhance={() => {
                    handleSubmit();
                    return async ({ update }) => {
                        await update();
                    };
                }}
            >
                {#if form?.error}
                    <Alert variant="destructive" class="mb-4">
                        <AlertCircle class="h-4 w-4" />
                        <AlertTitle>Error</AlertTitle>
                        <AlertDescription>{form.error}</AlertDescription>
                    </Alert>
                {/if}

                <div class="grid gap-4">
                    <div class="grid gap-2">
                        <Label for="username">Username</Label>
                        <Input
                            id="username"
                            name="username"
                            bind:value={username}
                            required
                            disabled={isSubmitting}
                            autocomplete="username"
                        />
                    </div>
                    <div class="grid gap-2">
                        <Label for="password">Password</Label>
                        <Input
                            id="password"
                            name="password"
                            type="password"
                            bind:value={password}
                            required
                            disabled={isSubmitting}
                            autocomplete="current-password"
                        />
                    </div>
                    <Button type="submit" class="w-full" disabled={isSubmitting}>
                        {isSubmitting ? 'Signing in...' : 'Sign in'}
                    </Button>
                    <div class="mt-4 text-center text-sm">
                        Don't have an account?
                        <a href="/signup" class="underline"> Create one </a>
                    </div>
                </div>
            </form>
        </CardContent>
    </Card>
</div>