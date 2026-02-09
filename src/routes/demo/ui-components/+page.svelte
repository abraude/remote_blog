<script lang="ts">
	import * as Card from '$lib/components/ui/card';
	import * as Input from '$lib/components/ui/input';
	import * as Button from '$lib/components/ui/button';
	import * as Checkbox from '$lib/components/ui/checkbox';
	import * as Select from '$lib/components/ui/select';
	import * as Label from '$lib/components/ui/label';

	let email = '';
	let password = '';
	let fullName = '';
	let role = '';
	let rememberMe = false;
	let newsletter = true;
	let termsAccepted = false;
</script>

<div class="flex min-h-screen items-center justify-center p-4">
	<Card.Root class="w-full max-w-md">
		<Card.Header>
			<Card.Title>UI Components Demo</Card.Title>
			<Card.Description>Explore shadcn-svelte UI components</Card.Description>
		</Card.Header>

		<Card.Content class="space-y-6">
			<form class="space-y-4" onsubmit={(e) => e.preventDefault()}>
				<div class="space-y-2">
					<Label.Root for="fullName">Full Name</Label.Root>
					<Input.Root id="fullName" bind:value={fullName} placeholder="John Doe" type="text" />
				</div>

				<div class="space-y-2">
					<Label.Root for="email">Email Address</Label.Root>
					<Input.Root id="email" bind:value={email} placeholder="john@example.com" type="email" />
				</div>

				<div class="space-y-2">
					<Label.Root for="password">Password</Label.Root>
					<Input.Root id="password" bind:value={password} placeholder="••••••••" type="password" />
				</div>

				<div class="space-y-2">
					<Label.Root for="role">Role</Label.Root>
					<Select.Root bind:value={role} type="single">
						<Select.Trigger id="role">
							{role ? role : 'Select a role'}
						</Select.Trigger>
						<Select.Content>
							<Select.Item value="admin">Administrator</Select.Item>
							<Select.Item value="editor">Editor</Select.Item>
							<Select.Item value="user">User</Select.Item>
							<Select.Item value="guest">Guest</Select.Item>
						</Select.Content>
					</Select.Root>
				</div>

				<div class="space-y-3">
					<div class="flex items-center gap-2">
						<Checkbox.Root bind:checked={rememberMe} />
						<Label.Root for="rememberMe">Remember me</Label.Root>
					</div>

					<div class="flex items-center gap-2">
						<Checkbox.Root bind:checked={newsletter} />
						<Label.Root for="newsletter">Subscribe to newsletter</Label.Root>
					</div>

					<div class="flex items-center gap-2">
						<Checkbox.Root bind:checked={termsAccepted} />
						<Label.Root for="termsAccepted">I accept the terms and conditions</Label.Root>
					</div>
				</div>

				<div class="flex gap-2 pt-2">
					<Button.Root type="submit" variant="default">Submit</Button.Root>
					<Button.Root type="button" variant="outline">Cancel</Button.Root>
					<Button.Root type="button" variant="ghost">Learn More</Button.Root>
				</div>
			</form>
		</Card.Content>

		<Card.Footer>
			<p class="text-sm text-muted-foreground">This is a demo of shadcn-svelte UI components.</p>
		</Card.Footer>
	</Card.Root>
</div>
