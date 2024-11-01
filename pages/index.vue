<script setup>

const inputTexts = ref()

const regex = ref()

const filteredData = ref([])

function extractCompanyName(text) {
    // Regex patterns for different name styles
    const handleRegex = /@([a-zA-Z0-9_.]+)/; // Social handle
    const businessKeywordRegex = /\b([A-Z][a-z]+(?:\s[A-Z][a-z]+)*(?:\s(?:Inc|LLC|Ltd|Corporation|Corp|Group|Solutions|Consulting|Agency|Studio|Labs|Global))?)\b/;
    const capitalizedWordsRegex = /\b([A-Z][a-z]+(?:\s[A-Z][a-z]+)*)\b/;

    // Priority check: Social handle -> Business keywords -> Capitalized words
    const handleMatch = text.match(handleRegex);
    const businessMatch = text.match(businessKeywordRegex);
    const capitalizedMatch = text.match(capitalizedWordsRegex);

    if (handleMatch) {
        return handleMatch[1]; // First social handle
    } else if (businessMatch) {
        return businessMatch[0]; // Business name with suffix
    } else if (capitalizedMatch) {
        return capitalizedMatch[0]; // General capitalized phrase as a fallback
    }
    return "Not provided";
}

function extractMails(text) {
  const coachData = [];
    
    // Regular expressions to match the desired data
    const nameRegex = /([A-Z][a-z]+(?: [A-Z][a-z]+)+)/g; // Matches names with at least two parts (e.g., "John Doe")
    const companyRegex = /\b([A-Z][a-z]+(?:\s[A-Z][a-z]+)*(?:\s(?:Inc|LLC|Ltd|Corporation|Corp|Group|Associates|Solutions|Consulting|Agency|Studio|Labs|Systems|Global))?)\b|@([a-zA-Z0-9_.]+)/g; 
    // Matches Instagram handles or similar patterns
    const websiteRegex = /(https?:\/\/[^\s]+)/g; // Matches URLs
    const emailRegex = /([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6})/g; // Matches email addresses
    const phoneRegex = /(\+?\d{1,4}[-. ]?\(?\d{1,4}?\)?[-. ]?\d{1,4}[-. ]?\d{1,9})/g; // Matches phone numbers
    
    // Split the text into lines
    const lines = text.split('\n');

    lines.forEach(line => {
        const nameMatches = line.match(nameRegex);
        const companyMatches = line.match(companyRegex);
        const websiteMatches = line.match(websiteRegex);
        const emailMatches = line.match(emailRegex);
        const phoneMatches = line.match(phoneRegex);
        
        if (nameMatches && nameMatches.length > 0 && emailMatches || websiteMatches) {
            coachData.push({
                fullName: nameMatches[0], // First name match
                companyName: companyMatches ? companyMatches[0].substring(1) : "Not provided", // Remove '@' from Instagram handle
                websiteURL: websiteMatches ? websiteMatches[0] : "Not provided",
                email: emailMatches ? emailMatches[0] : "Not provided",
                phoneNumber: phoneMatches ? phoneMatches[0] : "Not provided"
            });
        }
    });

    if (coachData.length > 0) {
      console.log(coachData)
      filteredData.value = coachData
    }
    
    // return coachData;
}
// watch(
//   () => inputTexts,
//   () => {
    
//   },
//   { deep: true }
// );
</script>

<template>
  <div class="text-lg max-w-7xl mx-auto min-h-screen bg-blue-500/40">

    <div >
      <h1 class="text-center text-4xl font-semibold">Email Extractor</h1>
    </div>
    <Textarea 
    v-model="inputTexts"/>

    <div class="flex justify-center my-10">

      <button @click="extractMails(inputTexts)" type="button" class="rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Extract</button>

    </div>

    <div >
      <table v-if="filteredData.length > 0" class="min-w-full bg-white border border-gray-300 rounded-lg shadow-md">
        <thead>
            <tr class="bg-gray-200">
                <th class="py-2 px-4 border-b border-gray-300 text-left">Full Name</th>
                <th class="py-2 px-4 border-b border-gray-300 text-left">Company Name</th>
                <th class="py-2 px-4 border-b border-gray-300 text-left">Website URL</th>
                <th class="py-2 px-4 border-b border-gray-300 text-left">Email</th>
                <th class="py-2 px-4 border-b border-gray-300 text-left">Phone Number</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for=" (data, index) in filteredData" class="hover:bg-gray-100">
                <td class="py-2 px-4 border-b border-gray-300">{{ data["fullName"] }}</td>
                <td class="py-2 px-4 border-b border-gray-300">{{ data["companyName"] }}</td>
                <td class="py-2 px-4 border-b border-gray-300"><a :href="data['websiteURL']" class="text-blue-500 hover:underline">{{ data["websiteURL"] }}</a></td>
                <td class="py-2 px-4 border-b border-gray-300">{{ data["email"] }}</td>
                <td class="py-2 px-4 border-b border-gray-300">{{ data["phoneNumber"] }}</td>
            </tr>
            
        </tbody>
    </table>
    </div>
  </div>
</template>

<style></style>
