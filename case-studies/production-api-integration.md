# Case Study: Production API Integration & Resilient Data Mapping

## 📌 Context
Integration of the production analytics API for the "M-Academy" platform. This phase focused on migrating from a mock-based environment to a real-time data stream for 800+ artist profiles and weekly industry charts.

## 🛠️ The Technical Challenges

### 1. Data Payload Asymmetry
The backend returned inconsistent data structures depending on the artist's origin (e.g., collaborations as flat strings vs. nested objects). 
- **Solution**: Engineered a **Data Transformation Layer** within the `SongsService` using an adapter pattern to normalize all incoming payloads into a strict TypeScript `Song` interface before reaching the UI components.

### 2. Multi-Tier Award Hierarchy
The platform requires displaying only the highest certification achieved by a track (Diamond > Platinum > Gold).
- **Solution**: Developed a priority-based **Award Resolver**. It evaluates an array of certifications and returns the dominant badge, handling complex strings like "2P" (Double Platinum) vs "G" (Gold) via regex and priority mapping.

### 3. Type Safety & ID Normalization
Numeric IDs from the API caused precision issues and inconsistent routing behavior.
- **Solution**: Implemented mandatory **String Normalization** for all unique identifiers at the mapping level. This ensured 100% stability in Angular's `@for` tracking and routing parameters.

## 🚀 Impact & Senior Decisions

- **Signal-Based Reactivity**: Leveraged **Angular Signals** for the chart data, ensuring high-performance UI updates and 0 "hydration flashes" during SSR.
- **Defensive Engineering**: Built a "Bunker Mode" toggle to switch back to high-fidelity mocks if the staging API is undergoing maintenance, ensuring 100% uptime for stakeholder demos.
```typescript
// Architectural Snippet: Normalizing asymmetrical artist data
private normalizeArtists(raw: ApiChartSong): { displayArtist: string; artistIds: string[] } {
  if (Array.isArray(raw.artists)) {
    return {
      displayArtist: raw.artists.map(a => a.name).join(', '),
      artistIds: raw.artists.map(a => String(a.slug || a.id))
    };
  }
  return { displayArtist: String(raw.artist), artistIds: [] };
}
