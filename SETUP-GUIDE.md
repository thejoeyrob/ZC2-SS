# ZC2 Arena Setup Guide

## Database Schema Migration

The unified player profile system requires three new columns in your Supabase `profiles` table. These columns store essential player data for the Zombie Smash arcade game.

### Required Columns

| Column | Type | Default | Purpose |
|--------|------|---------|---------|
| `profile_picture` | TEXT | NULL | Zombie boss ID selected as player avatar |
| `high_score` | INTEGER | 0 | Best Zombie Smash arcade game score |
| `selected_skin` | TEXT | 'gunmetal' | Currently selected console skin |

### How to Apply the Migration

#### Option 1: Using Supabase SQL Editor (Recommended)

1. Go to your Supabase project dashboard
2. Click **SQL Editor** in the left sidebar
3. Click **+ New Query** to create a new SQL query
4. Copy and paste the contents of `supabase-migration.sql` from this repository
5. Click **Run** to execute the migration
6. You should see confirmation that the columns were added

#### Option 2: Manual Column Addition via Dashboard

If you prefer to add columns through the UI:

1. Go to **Table Editor** → **profiles** table
2. Click the **+** button to add a new column
3. Add three columns with these settings:

   **Column 1: profile_picture**
   - Type: `text`
   - Default: NULL
   - Required: No

   **Column 2: high_score**
   - Type: `integer`
   - Default: 0
   - Required: No

   **Column 3: selected_skin**
   - Type: `text`
   - Default: 'gunmetal'
   - Required: No

### Verification

After migration, verify the columns exist by:

```sql
SELECT column_name, data_type, column_default
FROM information_schema.columns
WHERE table_name = 'profiles'
ORDER BY ordinal_position;
```

This query should show all three new columns in the `profiles` table.

## Code Integration

The application code is already configured to use these columns:

- **Profile Picture**: Automatically assigned a random zombie boss on signup (jordan, glowinghumanity, debo, caffeinatedsloth, fatamy, or drmantis), with user selection UI modal
- **High Score**: Updated when players submit Zombie Smash game scores
- **Selected Skin**: Persisted when players change their console skin in-game

## Unified Profile System

The system has been updated to use a single `profiles` table instead of separate `arcade_profiles`:

- ✅ Player profile created on signup (required before playing)
- ✅ Profile picture selection modal shown after account creation
- ✅ Random zombie profile assigned if user skips selection
- ✅ Game scores saved to unified profiles table
- ✅ Leaderboard queries use profiles table
- ✅ All references to old arcade_profiles removed

## Next Steps

1. Apply the database migration to your Supabase project
2. Deploy the latest code to Vercel
3. Test the signup flow to verify:
   - Profile creation completes successfully
   - Profile picture modal appears after signup
   - Random zombie is assigned if user skips selection
   - Game scores are properly saved and appear on leaderboard
